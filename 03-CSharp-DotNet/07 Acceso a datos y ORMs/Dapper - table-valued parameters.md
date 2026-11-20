---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: C# y .NET
tags: [csharp-dotnet]
---
# Dapper - table-valued parameters

Parte de: [[03-CSharp-DotNet/07 Acceso a datos y ORMs/_Índice Acceso a datos y ORMs|Acceso a datos y ORMs]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **table-valued parameter** (TVP) es un parámetro de SQL Server que contiene **una tabla completa**: varias filas con columnas tipadas. Permite mandar a un procedimiento almacenado una lista de IDs o un conjunto de partidas en una sola llamada, en lugar de hacer una llamada por fila o armar listas de texto. Se basa en un **tipo de tabla definido por el usuario** que creas una vez en la base.

### Cómo funciona
1. Creas el tipo en SQL Server con `CREATE TYPE ... AS TABLE`.
2. El procedimiento declara un parámetro de ese tipo con `READONLY` (los TVP no se pueden modificar dentro del procedimiento).
3. Desde .NET armas las filas (con un `DataTable`, un `IEnumerable<SqlDataRecord>` o un `DbDataReader`) y las mandas con `SqlDbType.Structured` indicando el nombre del tipo.
4. Dentro del procedimiento el TVP se usa como cualquier tabla: `JOIN`, `INSERT ... SELECT`, `MERGE`.

```mermaid
flowchart LR
  A[List de PartidaDto] --> B[DataTable con columnas iguales al tipo]
  B --> C["AsTableValuedParameter('dbo.TipoPartida')"]
  C --> D[EXEC dbo.Pedido_GuardarPartidas @Partidas]
  D --> E[INSERT ... SELECT FROM @Partidas]
```

### Ejemplo
Script de base (en tu carpeta de migraciones SQL):

```sql
CREATE TYPE dbo.ListaEnteros AS TABLE (Valor INT NOT NULL PRIMARY KEY);
GO
CREATE TYPE dbo.TipoPartida AS TABLE (
    Producto VARCHAR(30) NOT NULL,
    Cantidad INT NOT NULL,
    Precio   DECIMAL(18,2) NOT NULL);
GO
CREATE OR ALTER PROCEDURE dbo.Pedido_ObtenerVarios @Ids dbo.ListaEnteros READONLY AS
BEGIN
    SET NOCOUNT ON;
    SELECT p.Id, p.Codigo, p.Total
    FROM dbo.Pedido p JOIN @Ids i ON i.Valor = p.Id;
END
```

Desde Dapper:

```csharp
using System.Data;
using Dapper;

static DataTable ComoListaEnteros(IEnumerable<int> ids)
{
    var dt = new DataTable();
    dt.Columns.Add("Valor", typeof(int)); // mismo orden y tipo que el TYPE
    foreach (var id in ids.Distinct()) dt.Rows.Add(id);
    return dt;
}

var pedidos = await cn.QueryAsync<PedidoDto>(
    "dbo.Pedido_ObtenerVarios",
    new { Ids = ComoListaEnteros(ids).AsTableValuedParameter("dbo.ListaEnteros") },
    commandType: CommandType.StoredProcedure);
```

`AsTableValuedParameter` es un método de extensión de Dapper sobre `DataTable`. Para listas muy grandes, `IEnumerable<SqlDataRecord>` evita construir el `DataTable` completo en memoria, pero el `DataTable` es más sencillo de leer y suficiente casi siempre.

### Por qué un TVP en vez de `WHERE Id IN @Ids`
Dapper soporta `WHERE Id IN @Ids` con una lista: **expande** la lista a `@Ids1, @Ids2, ..., @IdsN`. Es cómodo, pero:

- **Un plan por tamaño de lista**: 3 IDs y 4 IDs generan textos SQL distintos, así que el servidor compila y guarda muchos planes.
- **Límite de 2,100 parámetros por petición** en SQL Server: una lista grande simplemente falla.
- **No funciona con procedimientos almacenados**, que es tu patrón en Dionisio; un procedimiento necesita un parámetro fijo.
- Con TVP el texto SQL es siempre el mismo, el plan se reutiliza, el tipo está validado por la base y puedes mandar **varias columnas** (partidas completas para un `INSERT` masivo dentro de una transacción).

Como alternativa sin tipo de usuario existen `STRING_SPLIT` o `OPENJSON` con un parámetro de texto, útiles si no puedes crear tipos; pierdes la validación de tipos y las estimaciones pueden ser peores.

### Cuándo usarlo y cuándo no
Úsalo para: listas de IDs a filtrar en procedimientos, inserción o actualización de varias filas en una sola llamada (partidas de un pedido, importaciones de Excel de Wee.Reporteria de tamaño moderado) y operaciones `MERGE`. Para cargas de decenas o cientos de miles de filas, `SqlBulkCopy` hacia una tabla de staging suele ser más rápido. Para dos o tres valores fijos, parámetros normales bastan.

### Errores comunes
- **Columnas del `DataTable` en otro orden** que el tipo: SQL Server mapea por posición, no por nombre; datos cruzados o error de conversión.
- **Olvidar `READONLY`** en el parámetro del procedimiento: error de compilación del procedimiento.
- **Cambiar el tipo**: no se puede `ALTER TYPE`; hay que eliminar los procedimientos que lo usan, recrear el tipo y recrearlos. Planea el versionado (`dbo.TipoPartida_v2`).
- **No dar permisos** `EXECUTE` sobre el tipo al usuario de la aplicación: `GRANT EXECUTE ON TYPE::dbo.ListaEnteros TO usuario_app`.
- **Duplicados con `PRIMARY KEY`** en el tipo: aplica `Distinct()` antes.

### Para profundizar
- [Parámetros con valores de tabla (Microsoft Learn)](https://learn.microsoft.com/sql/relational-databases/tables/use-table-valued-parameters-database-engine)
- [Table-valued parameters en ADO.NET (Microsoft Learn)](https://learn.microsoft.com/dotnet/framework/data/adonet/sql/table-valued-parameters)
- [Dapper en GitHub](https://github.com/DapperLib/Dapper)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es un TVP?
> Un parámetro de tipo tabla de SQL Server (`CREATE TYPE dbo.IdLista AS TABLE (Id INT PRIMARY KEY)`) que permite mandar muchas filas en un solo parámetro a un SP.

> [!question]- ¿Cómo se manda desde Dapper?
> ```csharp
> var dt = new DataTable(); dt.Columns.Add("Id", typeof(int));
> foreach (var id in ids) dt.Rows.Add(id);
> await cn.ExecuteAsync("dbo.Pedidos_Cancelar", new { Ids = dt.AsTableValuedParameter("dbo.IdLista") }, commandType: CommandType.StoredProcedure);
> ```

> [!question]- ¿Por qué un TVP en vez de `WHERE Id IN @Ids` con Dapper?
> Dapper expande la lista en un parámetro por elemento: con muchos ids rebasa el límite de ~2100 parámetros de SQL Server y genera un plan distinto por cada tamaño.

## Práctica

- [[Lab 9 - Dapper con stored procedures y transacción]]

