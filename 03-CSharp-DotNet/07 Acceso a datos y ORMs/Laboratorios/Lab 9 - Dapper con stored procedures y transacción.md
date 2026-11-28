---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: C# y .NET
herramienta: SQL Server Developer o Express (o LocalDB), SSMS o Azure Data Studio, .NET 10 SDK
tags: [csharp-dotnet, laboratorio]
contenido: completo
---
# Lab 9 - Dapper con stored procedures y transacción

Parte de: [[03-CSharp-DotNet/07 Acceso a datos y ORMs/_Índice Acceso a datos y ORMs|Acceso a datos y ORMs]]

**Cubre:** [[Dapper]] · [[Dapper - transacciones]] · [[Dapper - QueryMultiple y multi-mapping]] · [[Dapper - table-valued parameters]] · [[SQL en archivos con una interfaz por consulta]] · [[Parametrización y SQL injection]] · [[Connection pooling]] · [[ADO.NET]] · [[Problema N+1]] · [[Repository]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Implementar un repositorio de pedidos con Dapper contra SQL Server: stored procedures, una transacción con varios comandos, `QueryMultiple` para evitar N+1 y un TVP para operaciones por lote.

## Preparación (SQL)

```sql
CREATE DATABASE LabDapper;
GO
USE LabDapper;
CREATE TABLE dbo.Pedido (Id INT IDENTITY PRIMARY KEY, Cliente NVARCHAR(50) NOT NULL, Estado VARCHAR(20) NOT NULL DEFAULT 'Abierto', Total DECIMAL(12,2) NOT NULL DEFAULT 0);
CREATE TABLE dbo.PedidoLinea (Id INT IDENTITY PRIMARY KEY, PedidoId INT NOT NULL REFERENCES dbo.Pedido(Id), Producto VARCHAR(20) NOT NULL, Cantidad INT NOT NULL CHECK (Cantidad > 0), Precio DECIMAL(12,2) NOT NULL);
CREATE TYPE dbo.IdLista AS TABLE (Id INT PRIMARY KEY);
```

## Pasos

1. Escribe los SPs: `dbo.Pedido_Crear` (regresa el id con `SCOPE_IDENTITY()`), `dbo.PedidoLinea_Agregar`, `dbo.Pedido_ActualizarTotal`, `dbo.Pedido_Obtener` (dos `SELECT`: cabecera y líneas) y `dbo.Pedidos_Cancelar(@Ids dbo.IdLista READONLY)`.
2. Crea una consola o API con `Microsoft.Data.SqlClient` y `Dapper`. Guarda la cadena de conexión en User Secrets.
3. Sigue tu patrón: cada llamada a SP en su propio `.sql` embebido (`EXEC dbo.Pedido_Crear @Cliente`) o con `CommandType.StoredProcedure`, con su record de campos y su interfaz.
4. `PedidoRepositorio.CrearAsync(cliente, lineas)` debe crear el pedido, insertar las líneas y actualizar el total **en una sola transacción**.
5. `ObtenerAsync(id)` usa `QueryMultipleAsync` para traer cabecera y líneas en una ida.
6. `CancelarAsync(ids)` manda un TVP.
7. Escribe un intento de inyección: busca pedidos por cliente con el texto `x' OR 1=1 --`.

## Criterios de terminado

- [ ] Crear un pedido con 3 líneas deja 1 fila en `Pedido`, 3 en `PedidoLinea` y el `Total` correcto.
- [ ] Si la tercera línea tiene `Cantidad = 0`, falla el `CHECK`, y **no** queda ni el pedido ni las dos primeras líneas (rollback).
- [ ] `ObtenerAsync` ejecuta una sola ida a la BD (lo confirmo con Extended Events/Profiler o contando comandos).
- [ ] `CancelarAsync([1, 2, 3])` cambia las 3 filas a `Cancelado` con un solo comando.
- [ ] La búsqueda con `x' OR 1=1 --` regresa 0 filas.
- [ ] No hay SQL concatenado con datos del usuario en ninguna parte del código.
- [ ] Todas las conexiones se abren con `await using`.

## Solución

> [!success]- Stored procedures
> ```sql
> CREATE OR ALTER PROCEDURE dbo.Pedido_Crear @Cliente NVARCHAR(50) AS
> BEGIN SET NOCOUNT ON; INSERT dbo.Pedido (Cliente) VALUES (@Cliente); SELECT CAST(SCOPE_IDENTITY() AS INT); END;
> GO
> CREATE OR ALTER PROCEDURE dbo.PedidoLinea_Agregar @PedidoId INT, @Producto VARCHAR(20), @Cantidad INT, @Precio DECIMAL(12,2) AS
> BEGIN SET NOCOUNT ON; INSERT dbo.PedidoLinea (PedidoId, Producto, Cantidad, Precio) VALUES (@PedidoId, @Producto, @Cantidad, @Precio); END;
> GO
> CREATE OR ALTER PROCEDURE dbo.Pedido_ActualizarTotal @PedidoId INT AS
> BEGIN SET NOCOUNT ON;
>   UPDATE p SET Total = (SELECT COALESCE(SUM(Cantidad * Precio), 0) FROM dbo.PedidoLinea WHERE PedidoId = @PedidoId)
>   FROM dbo.Pedido p WHERE p.Id = @PedidoId; END;
> GO
> CREATE OR ALTER PROCEDURE dbo.Pedido_Obtener @Id INT AS
> BEGIN SET NOCOUNT ON;
>   SELECT Id, Cliente, Estado, Total FROM dbo.Pedido WHERE Id = @Id;
>   SELECT Id, Producto, Cantidad, Precio FROM dbo.PedidoLinea WHERE PedidoId = @Id; END;
> GO
> CREATE OR ALTER PROCEDURE dbo.Pedidos_Cancelar @Ids dbo.IdLista READONLY AS
> BEGIN SET NOCOUNT ON; UPDATE p SET Estado = 'Cancelado' FROM dbo.Pedido p JOIN @Ids i ON i.Id = p.Id; END;
> ```

> [!success]- Transacción
> ```csharp
> public async Task<int> CrearAsync(string cliente, IReadOnlyList<LineaNueva> lineas, CancellationToken ct)
> {
>     await using var cn = new SqlConnection(_cs);
>     await cn.OpenAsync(ct);
>     await using var tx = await cn.BeginTransactionAsync(ct);
>     var sp = CommandType.StoredProcedure;
>     var id = await cn.ExecuteScalarAsync<int>(new CommandDefinition("dbo.Pedido_Crear", new { Cliente = cliente }, tx, commandType: sp, cancellationToken: ct));
>     foreach (var l in lineas)
>         await cn.ExecuteAsync(new CommandDefinition("dbo.PedidoLinea_Agregar", new { PedidoId = id, l.Producto, l.Cantidad, l.Precio }, tx, commandType: sp, cancellationToken: ct));
>     await cn.ExecuteAsync(new CommandDefinition("dbo.Pedido_ActualizarTotal", new { PedidoId = id }, tx, commandType: sp, cancellationToken: ct));
>     await tx.CommitAsync(ct);
>     return id;
> }
> ```
> Si cualquier comando lanza, no se llega a `CommitAsync` y el `DisposeAsync` de la transacción hace rollback.

> [!success]- QueryMultiple y TVP
> ```csharp
> using var g = await cn.QueryMultipleAsync(new CommandDefinition("dbo.Pedido_Obtener", new { Id = id }, commandType: CommandType.StoredProcedure, cancellationToken: ct));
> var cab = await g.ReadSingleOrDefaultAsync<PedidoFila>();
> var lineas = (await g.ReadAsync<LineaFila>()).ToList();
>
> var dt = new DataTable(); dt.Columns.Add("Id", typeof(int));
> foreach (var i in ids) dt.Rows.Add(i);
> await cn.ExecuteAsync("dbo.Pedidos_Cancelar", new { Ids = dt.AsTableValuedParameter("dbo.IdLista") }, commandType: CommandType.StoredProcedure);
> ```

## Notas personales

_Qué me costó, errores que cometí, dudas._
