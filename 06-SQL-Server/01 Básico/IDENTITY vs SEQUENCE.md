---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: SQL Server
tags: [sql-server]
---
# IDENTITY vs SEQUENCE

Parte de: [[06-SQL-Server/01 Básico/_Índice Básico|Básico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Ambos generan números automáticos para claves:

- **`IDENTITY`** es una **propiedad de una columna**: cada `INSERT` en esa tabla obtiene el siguiente valor. Hay como máximo una por tabla.
- **`SEQUENCE`** (SQL Server 2012+) es un **objeto independiente** del esquema que entrega números con `NEXT VALUE FOR`. No pertenece a ninguna tabla, así que puedes compartirlo entre varias, pedir el valor **antes** del `INSERT` o reservar rangos.

### Cómo funciona
**IDENTITY(semilla, incremento)**, por ejemplo `int IDENTITY(1,1)`:

- El valor se asigna al insertar; no puedes actualizarlo después.
- Para recuperar el valor recién generado:
  - **`SCOPE_IDENTITY()`**: último valor generado en *tu* sesión y *tu* ámbito (el lote o procedimiento actual). Es la opción correcta.
  - `@@IDENTITY`: último valor de tu sesión en **cualquier** ámbito; si un trigger insertó en otra tabla con identity, te devuelve ese valor. Peligroso.
  - `IDENT_CURRENT('tabla')`: último valor de esa tabla en **cualquier sesión**. Con concurrencia te puede dar el id de otro usuario.
  - Mejor aún: la cláusula **`OUTPUT INSERTED.Id`**, que funciona también con inserciones de varias filas (ver [[OUTPUT]]).
- Para insertar un valor específico (migraciones, cargar catálogos con ids fijos): `SET IDENTITY_INSERT tabla ON;`, insertar con lista de columnas explícita, y `OFF`. Solo una tabla por sesión puede tenerlo activo.
- `DBCC CHECKIDENT('tabla', RESEED, n)` reajusta el siguiente valor.

**Huecos**: ninguno de los dos garantiza números consecutivos. Hay huecos porque:

1. Un `INSERT` que falla o una transacción que hace `ROLLBACK` **consume** el valor y no lo devuelve (así no se serializan las inserciones concurrentes).
2. Por rendimiento, SQL Server **cachea** valores en memoria; si el servicio se reinicia de forma inesperada, se pierden los cacheados (saltos de hasta 1,000 en `int`). Se puede desactivar con `ALTER DATABASE SCOPED CONFIGURATION SET IDENTITY_CACHE = OFF` (2017+) a costa de rendimiento, y en `SEQUENCE` con `NO CACHE`.
3. Borrados de filas.

Por eso, si el negocio exige folios fiscales sin huecos, no uses ni IDENTITY ni SEQUENCE: necesitas una tabla de folios actualizada dentro de la transacción (con su costo de concurrencia).

**SEQUENCE**:

```sql
CREATE SEQUENCE ventas.SeqFolio AS bigint START WITH 1 INCREMENT BY 1 CACHE 50;
SELECT NEXT VALUE FOR ventas.SeqFolio;            -- obtener antes de insertar
-- Como DEFAULT de una o varias tablas
ALTER TABLE ventas.Notas ADD CONSTRAINT DF_Notas_Folio DEFAULT (NEXT VALUE FOR ventas.SeqFolio) FOR Folio;
-- Reservar un rango de 100 valores de una vez
EXEC sys.sp_sequence_get_range @sequence_name = N'ventas.SeqFolio', @range_size = 100,
     @range_first_value = @primero OUTPUT;
```

### Ejemplo
Procedimiento de Itzamna.Dionisio que crea un cliente y devuelve su id, llamado con Dapper:

```sql
CREATE OR ALTER PROCEDURE api.Cliente_Crear
    @Nombre nvarchar(100), @Ciudad nvarchar(60) = NULL
AS
SET NOCOUNT ON;
INSERT INTO ventas.Clientes (Nombre, Ciudad)
OUTPUT INSERTED.ClienteId                 -- devuelve el id como resultado
VALUES (@Nombre, @Ciudad);
-- Alternativa clásica: INSERT ...; SELECT CAST(SCOPE_IDENTITY() AS int);
```

```csharp
int nuevoId = await conexion.ExecuteScalarAsync<int>(
    "api.Cliente_Crear", new { Nombre = "Gabriela Paz", Ciudad = "Puebla" },
    commandType: CommandType.StoredProcedure);
```

Cuándo brilla `SEQUENCE`: si un pedido y sus renglones se arman en C# y necesitas el `PedidoId` antes de mandar todo junto (por ejemplo con un [[Table-valued parameters]]), pides el número primero; o si dos tablas (`NotasCredito` y `NotasCargo`) deben compartir una numeración única.

### Cuándo usarlo y cuándo no
- **IDENTITY** para la gran mayoría de claves sustitutas de una sola tabla: simple, rápido, creciente (bueno para el índice clustered).
- **SEQUENCE** cuando necesitas el valor antes del `INSERT`, numeración compartida entre tablas, reciclar con `CYCLE`, o reservar rangos.
- **Ninguno** para folios legales sin huecos.
- Si la clave se genera fuera de la base (sistemas distribuidos), considera [[uniqueidentifier]].

### Errores comunes
1. **Usar `@@IDENTITY`** y recibir el id de una tabla de auditoría llenada por un trigger.
2. **`IDENT_CURRENT` o `MAX(Id) + 1`** con concurrencia: dos sesiones obtienen el mismo número.
3. **Asumir que no hay huecos** y construir lógica de negocio sobre ello ("el pedido 1050 falta, alguien lo borró").
4. **Olvidar `SET IDENTITY_INSERT ... OFF`** tras una migración, o no listar columnas en el `INSERT` (error 545).
5. **`int` que se agota**: a 2,147,483,647 el siguiente `INSERT` falla. Si la tabla crecerá mucho (logs, eventos), usa `bigint` desde el inicio.

### Para profundizar
- [IDENTITY (propiedad)](https://learn.microsoft.com/sql/t-sql/statements/create-table-transact-sql-identity-property)
- [Números de secuencia](https://learn.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers)
- [SCOPE_IDENTITY (Transact-SQL)](https://learn.microsoft.com/sql/t-sql/functions/scope-identity-transact-sql)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre `IDENTITY` y `SEQUENCE`?
> `IDENTITY` es una propiedad de una columna de una tabla. `SEQUENCE` es un objeto independiente (`NEXT VALUE FOR ventas.SeqFolio`): puede compartirse entre tablas, pedirse antes de insertar y reiniciarse o ciclarse.

> [!question]- ¿Por qué hay huecos en los valores de `IDENTITY`?
> Las transacciones revertidas consumen valores, y tras un reinicio inesperado se pierde la caché de valores (saltos de ~1000). No uses `IDENTITY` para folios que deban ser consecutivos sin huecos.

> [!question]- `SCOPE_IDENTITY()`, `@@IDENTITY` e `IDENT_CURRENT('t')`: ¿cuál usar tras un `INSERT`?
> `SCOPE_IDENTITY()` (último valor en tu sesión y ámbito) u `OUTPUT inserted.Id`. `@@IDENTITY` puede devolver el valor generado por un trigger en otra tabla; `IDENT_CURRENT` ve inserciones de otras sesiones.

> [!question]- ¿Cómo insertas un valor específico en una columna `IDENTITY` (p. ej. al migrar datos)?
> `SET IDENTITY_INSERT ventas.Clientes ON;` → `INSERT` con la lista de columnas incluyendo el id → `SET IDENTITY_INSERT ventas.Clientes OFF;`. Solo una tabla por sesión puede tenerlo activo.

## Práctica

- [[Lab 1 - Base de práctica Tienda en SQL Server]]

