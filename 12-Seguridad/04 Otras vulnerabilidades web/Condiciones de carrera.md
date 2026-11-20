---
proyecto: Conocimiento
tipo: concepto
nivel: avanzado
progreso: pendiente
estado: pendiente
area: Seguridad
tags: [seguridad]
---
# Condiciones de carrera

Parte de: [[12-Seguridad/04 Otras vulnerabilidades web/_Índice Otras vulnerabilidades web|Otras vulnerabilidades web]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una condición de carrera ocurre cuando el resultado de una operación depende del orden en que se ejecutan peticiones concurrentes. Tiene impacto de seguridad cuando dos o más peticiones simultáneas **pasan una verificación antes de que alguna actualice el estado**: canjear dos veces un cupón de un solo uso, retirar más saldo del disponible, aceptar dos veces una invitación con límite, o saltarse un límite de intentos. El atacante no necesita romper nada; solo manda muchas peticiones idénticas al mismo tiempo.

### Cómo funciona: TOCTOU
El patrón clásico se llama **TOCTOU** (*time-of-check to time-of-use*): el estado cambia entre el momento en que lo verificas y el momento en que lo usas.

```mermaid
sequenceDiagram
    participant P1 as Petición 1
    participant P2 as Petición 2
    participant DB as SQL Server
    P1->>DB: SELECT Usado FROM Cupon WHERE Id=9 (0)
    P2->>DB: SELECT Usado FROM Cupon WHERE Id=9 (0)
    P1->>DB: UPDATE Cupon SET Usado=1; aplica descuento
    P2->>DB: UPDATE Cupon SET Usado=1; aplica descuento otra vez
```

El código "parece" correcto leído de arriba abajo: primero revisa, luego actualiza. El problema es la ventana entre ambos pasos, que con un servidor ocupado o con peticiones disparadas en paralelo es suficiente. Herramientas como el Repeater de Burp permiten mandar varias peticiones en paralelo para probarlo en tus propios sistemas.

### Cómo se detecta
- **Revisión de código**: busca el patrón "leer, decidir en C#, escribir" sobre recursos limitados (saldo, inventario, cupones, tokens de un solo uso, contadores de intentos).
- **Pruebas de carga dirigidas**: una prueba de integración que lanza 20 peticiones concurrentes al mismo endpoint y comprueba que solo una tuvo éxito.
- **Datos imposibles en producción**: saldos negativos, cupones con dos canjes, más reservaciones que lugares.

### Cómo se previene en SQL Server
La regla: **la verificación y la actualización deben ser una sola operación atómica**, o estar protegidas por un bloqueo que impida que otra transacción lea el mismo estado mientras tanto.

1. **Actualización condicional**: poner la condición en el `WHERE` y revisar cuántas filas cambió.
2. **Restricciones de la base**: un índice único (por ejemplo `UNIQUE (CuponId)` en la tabla de canjes) o un `CHECK (Saldo >= 0)`; la base rechaza el segundo intento aunque el código falle.
3. **Bloqueos explícitos** cuando necesitas leer y luego decidir: `SELECT ... WITH (UPDLOCK, HOLDLOCK)` dentro de una transacción, que hace que la segunda transacción espere (ver [[Bloqueos (locks)]] y [[SERIALIZABLE y bloqueos de rango]]).
4. **Concurrencia optimista** con `rowversion` cuando el conflicto es raro y prefieres detectarlo y reintentar ([[Concurrencia optimista]]).

### Ejemplo
Canje de cupón en Itzamna.Dionisio, con el SQL en su archivo y una interfaz por consulta.

```sql
-- Sql/Cupones/CanjearCupon.sql (procedimiento dbo.CanjearCupon)
CREATE OR ALTER PROCEDURE dbo.CanjearCupon
    @CuponId INT,
    @UsuarioId INT,
    @TenantId INT
AS
BEGIN
    SET NOCOUNT ON;
    SET XACT_ABORT ON;

    -- Verificación y cambio en una sola sentencia atómica
    UPDATE dbo.Cupon
       SET Usado = 1, UsadoPor = @UsuarioId, UsadoEn = SYSUTCDATETIME()
     WHERE Id = @CuponId
       AND TenantId = @TenantId
       AND Usado = 0
       AND VigenteHasta > SYSUTCDATETIME();

    -- Con NOCOUNT ON, Dapper no recibe filas afectadas; las devolvemos explícitamente
    SELECT @@ROWCOUNT AS Filas;
END
```

```csharp
public interface ICanjearCupon
{
    Task<bool> EjecutarAsync(int cuponId, int usuarioId, int tenantId, CancellationToken ct);
}

public sealed class CanjearCupon(IDbConnectionFactory fabrica) : ICanjearCupon
{
    public async Task<bool> EjecutarAsync(int cuponId, int usuarioId, int tenantId, CancellationToken ct)
    {
        await using var cn = await fabrica.AbrirAsync(ct);
        var filas = await cn.ExecuteScalarAsync<int>(new CommandDefinition(
            "dbo.CanjearCupon",
            new { CuponId = cuponId, UsuarioId = usuarioId, TenantId = tenantId },
            commandType: CommandType.StoredProcedure,
            cancellationToken: ct));
        return filas == 1; // 0 = ya usado, vencido o no es de este tenant
    }
}
```

El controller responde `409 Conflict` con ProblemDetails (`codigo = "CUPON_NO_DISPONIBLE"`) cuando `EjecutarAsync` devuelve `false`. Aunque lleguen 50 peticiones a la vez, solo una encuentra `Usado = 0`: SQL Server toma un bloqueo de actualización sobre la fila y las demás ven el valor ya cambiado.

Para saldos: `UPDATE Cuenta SET Saldo = Saldo - @Monto WHERE Id = @Id AND Saldo >= @Monto`, más un `CHECK (Saldo >= 0)` como red de seguridad.

### Más allá de la base
- **Caché o memoria**: un `lock` en C# solo protege dentro de un proceso; con varias instancias en Azure no sirve. El estado compartido debe vivir en la base o en un almacén con operaciones atómicas.
- **Archivos**: verificar que un archivo existe y luego abrirlo es otro TOCTOU; abre directamente y maneja la excepción.
- **Idempotencia**: para operaciones de pago, una llave de idempotencia con índice único evita dobles cargos por reintentos ([[Idempotencia]]).

### Errores comunes
- **Confiar en que "es muy rápido"**: la ventana existe y se puede ampliar mandando peticiones en paralelo.
- **Usar `SELECT` y luego `UPDATE` en una transacción con aislamiento por defecto** creyendo que eso bloquea: `READ COMMITTED` no impide que otra transacción lea el mismo valor antes de tu `UPDATE`.
- **Olvidar `SET NOCOUNT ON` y su efecto en Dapper**: con NOCOUNT activo, `ExecuteAsync` no informa filas afectadas; devuelve el conteo explícito.
- **Resolverlo en el front** deshabilitando el botón: el atacante no usa tu botón.

### Para profundizar
- [PortSwigger Academy - Race conditions](https://portswigger.net/web-security/race-conditions)
- [CWE-367: Time-of-check Time-of-use (TOCTOU) Race Condition](https://cwe.mitre.org/data/definitions/367.html)
- [Microsoft Learn - Sugerencias de tabla (UPDLOCK, HOLDLOCK)](https://learn.microsoft.com/sql/t-sql/queries/hints-transact-sql-table)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es una condición de carrera con impacto de seguridad?
> Cuando dos peticiones simultáneas pasan una verificación antes de que alguna actualice el estado: por ejemplo, canjear un cupón de un solo uso dos veces o retirar más saldo del disponible.

> [!question]- ¿Cómo se previene en SQL Server?
> Haciendo la verificación y la actualización en una sola sentencia atómica (`UPDATE ... SET Usado = 1 WHERE Id = @Id AND Usado = 0` y revisar filas afectadas), con restricciones únicas, o con transacciones y bloqueos adecuados (`UPDLOCK`, `HOLDLOCK`).

> [!question]- ¿Qué es TOCTOU?
> Time-of-check to time-of-use: el estado cambia entre el momento en que se verifica y el momento en que se usa.

