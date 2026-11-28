---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Fundamentos
tags: [fundamentos]
contenido: completo
---
# Clean code

Parte de: [[02-Fundamentos/08 Computación general/_Índice Computación general|Computación general]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Código limpio** es código que **otra persona (o tú en seis meses) puede leer, entender y cambiar con seguridad** sin esfuerzo excesivo. El término lo popularizó Robert C. Martin con su libro *Clean Code* (2008), pero la idea es más amplia: el código se lee muchas más veces de las que se escribe, así que optimizar su lectura es optimizar el costo del software.

### Cómo funciona
Rasgos que lo caracterizan:

- **Nombres que revelan intención**: `diasDeAtraso` en lugar de `d`; `ObtenerClientesConSaldoVencido()` en lugar de `GetData2()`. Si necesitas un comentario para explicar un nombre, cambia el nombre.
- **Funciones pequeñas y con un solo nivel de abstracción**: o coordinan pasos o hacen un detalle, no ambos.
- **Pocos parámetros** y sin banderas booleanas que cambian el comportamiento (`Exportar(true, false)` no dice nada).
- **Sin duplicación de conocimiento** (ver [[DRY, KISS y YAGNI]]).
- **Manejo de errores claro**: excepciones o resultados explícitos, no códigos mágicos ni `catch {}` vacíos.
- **Consistencia**: mismo estilo en todo el repo, idealmente automatizado con `.editorconfig`, `dotnet format`, ESLint y Prettier.
- **Pruebas** que documentan el comportamiento y permiten refactorizar sin miedo.

**Comentarios.** El código debe explicar el *qué* y el *cómo*; los comentarios valen la pena para el **porqué** que el código no puede expresar:
- Decisiones no obvias ("usamos `READ COMMITTED SNAPSHOT` porque el reporte bloqueaba capturas").
- Advertencias ("el SAT rechaza más de 2,000 conceptos por CFDI").
- Referencias a requisitos legales, bugs o RFC.
- Documentación de API pública (`///` en C#, JSDoc).
No valen la pena los que repiten el código (`// incrementa i`), el código comentado "por si acaso" (para eso está Git) o los que mienten porque nadie los actualizó.

### Ejemplo
Antes (Wee.Reporteria):

```csharp
public List<R> Get(int t, bool f)
{
    var l = new List<R>();
    foreach (var x in _db.Query<R>("SELECT * FROM Movs"))
    {
        // si es tipo 3 y no está cancelado
        if (x.T == 3 && x.E != 9 && (f ? x.Fecha > DateTime.Now.AddDays(-30) : true))
            l.Add(x);
    }
    return l;
}
```

Después:

```csharp
private const int TipoCargo = 3;
private const int EstatusCancelado = 9;

public async Task<IReadOnlyList<Movimiento>> ObtenerCargosVigentesAsync(
    DateOnly? desde, CancellationToken ct)
{
    // El filtrado vive en ObtenerCargosVigentes.sql: la tabla tiene millones de filas
    var movimientos = await _query.EjecutarAsync(desde, ct);
    return movimientos;
}
```

```sql
-- ObtenerCargosVigentes.sql
SELECT Id, ClienteId, Fecha, Importe
FROM dbo.Movimientos
WHERE Tipo = 3            -- cargo
  AND Estatus <> 9        -- cancelado
  AND (@Desde IS NULL OR Fecha >= @Desde);
```

Se eliminaron el `bool` ambiguo, los números mágicos, el `SELECT *` y el filtrado en memoria; el comentario que queda explica un porqué.

### Cuándo usarlo y cuándo no
- En todo código que vaya a vivir y mantenerse: APIs, librerías, scripts que se ejecutan en producción.
- Un script de una sola vez o un prototipo que vas a tirar no necesita el mismo cuidado (pero los prototipos suelen sobrevivir; cuidado).
- **Riesgo de seguir las reglas al pie de la letra:** convertirlas en dogma. Funciones de 3 líneas por obligación fragmentan la lógica y obligan a saltar entre 15 métodos para entender algo simple; interfaces y capas para "desacoplar" lo que nunca cambia; renombrar todo un módulo por gusto. Las reglas son heurísticas: el criterio es si el código queda **más fácil de entender y cambiar para tu equipo**, no si cumple una lista.

### Errores comunes
- **Abreviaturas privadas** (`cte`, `mov`, `prc`): cada quien las interpreta distinto.
- **Funciones con efectos secundarios ocultos**: `ValidarCliente()` que además guarda en la base.
- **Comentarios desactualizados**: peor que no tenerlos. Mantenlos o bórralos.
- **Limpiar sin pruebas**: refactorizas y rompes algo sin darte cuenta (ver [[Code smells y refactorización]]).
- **Mezclar limpieza masiva con funcionalidad** en el mismo PR: imposible de revisar.

### Para profundizar
- [Convenciones de codificación de C# (Microsoft Learn)](https://learn.microsoft.com/dotnet/csharp/fundamentals/coding-style/coding-conventions)
- [Reglas de estilo de código y .editorconfig en .NET](https://learn.microsoft.com/dotnet/fundamentals/code-analysis/code-style-rule-options)
- [Catálogo de refactorizaciones de Martin Fowler](https://refactoring.com/catalog/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué caracteriza al código limpio?
> Nombres que revelan intención, funciones pequeñas que hacen una cosa, sin duplicación, manejo de errores claro, pocos comentarios porque el código se explica, y pruebas.

> [!question]- ¿Cuándo sí vale la pena un comentario?
> Para explicar **por qué** (una decisión, una regla del negocio, una limitación externa), no **qué** hace el código.

> [!question]- ¿Qué riesgo tiene seguir reglas de clean code al pie de la letra?
> Fragmentar en demasiadas funciones diminutas o abstracciones que dificultan seguir el flujo. Son guías para la legibilidad del equipo, no reglas absolutas.

