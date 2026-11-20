---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: SQL Server
herramienta: SQL Server Developer o Express + SSMS; opcional .NET SDK con Dapper
tags: [sql-server, laboratorio]
---
# Lab 4 - Stored procedure transaccional con TRY CATCH y XACT_ABORT

Parte de: [[06-SQL-Server/02 Intermedio/_Índice Intermedio|Intermedio]]

**Cubre:** [[Stored procedures]] · [[SET NOCOUNT y SET XACT_ABORT]] · [[TRY CATCH y THROW]] · [[Parámetros de salida y RETURN]] · [[Table-valued parameters]] · [[CREATE OR ALTER y scripts idempotentes]] · [[Transacciones (BEGIN, COMMIT y ROLLBACK)]] · [[Propiedades ACID]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Escribir `ventas.Pedido_Crear`, un procedimiento que crea un pedido con sus renglones en **una sola transacción**, valida la entrada, lanza errores de negocio con `THROW 504xx` y nunca deja pedidos a medias. Usa la base Tienda ([[Lab 1 - Base de práctica Tienda en SQL Server]]).

## Pasos

1. Crea el tipo tabla `ventas.PedidoItem (ProductoId int PK, Cantidad int > 0)`.
2. Crea con `CREATE OR ALTER` el procedimiento `ventas.Pedido_Crear @ClienteId int, @Items ventas.PedidoItem READONLY, @PedidoId int OUTPUT` que:
   - empiece con `SET NOCOUNT ON; SET XACT_ABORT ON;`
   - lance `50400` si no hay renglones, `50404` si el cliente o algún producto no existe;
   - inserte el pedido (fecha de hoy UTC) y los renglones con el **precio de lista** dentro de una transacción;
   - en el `CATCH` haga `ROLLBACK` si hay transacción y relance con `THROW;`.
3. Prueba el caso feliz: cliente 6, productos 1 (×2) y 5 (×1). Lee `@PedidoId`.
4. Prueba los errores de validación (sin renglones, cliente 99, producto 999) y anota el número de cada error.
5. **Prueba de atomicidad**: pide 500 unidades del producto 2. El tipo tabla lo acepta (solo exige > 0), pero el `CHECK` de la tabla (1–100) falla **después** de insertar el pedido. Comprueba que no quedó el pedido.
6. **Sin red de seguridad**: en un script suelto (sin procedimiento), con `SET XACT_ABORT OFF` y sin `TRY/CATCH`, haz `BEGIN TRAN`, inserta un pedido, inserta un renglón con cantidad 500 y `COMMIT`. Observa qué quedó. Repite con `SET XACT_ABORT ON`.
7. (Opcional) Llama el procedimiento desde una app de consola con Dapper usando `AsTableValuedParameter` y traduce 50400/50404 a mensajes.

## Criterios de terminado

- [ ] El caso feliz crea un pedido nuevo con 2 renglones a 1200.00 y 150.00, y `@PedidoId` trae su id.
- [ ] Sin renglones → error 50400; cliente 99 o producto 999 → error 50404; ningún caso crea filas.
- [ ] Con 500 unidades el error es el 547 (CHECK) y `SELECT COUNT(*) FROM ventas.Pedidos` no cambia.
- [ ] En el paso 6 con `XACT_ABORT OFF` queda un pedido **sin renglones** (huérfano); con `ON` no queda nada.
- [ ] Ejecutar el script del procedimiento dos veces no da error (idempotente).
- [ ] Puedo explicar por qué `XACT_ABORT ON` también protege ante un *timeout* del cliente.

## Solución

> [!success]- Tipo y procedimiento
> ```sql
> IF TYPE_ID('ventas.PedidoItem') IS NULL
>     CREATE TYPE ventas.PedidoItem AS TABLE (
>         ProductoId int NOT NULL PRIMARY KEY,
>         Cantidad   int NOT NULL CHECK (Cantidad > 0)
>     );
> GO
> CREATE OR ALTER PROCEDURE ventas.Pedido_Crear
>     @ClienteId int,
>     @Items     ventas.PedidoItem READONLY,
>     @PedidoId  int OUTPUT
> AS
> BEGIN
>     SET NOCOUNT ON;
>     SET XACT_ABORT ON;
>     BEGIN TRY
>         IF NOT EXISTS (SELECT 1 FROM @Items)
>             THROW 50400, N'El pedido no tiene productos.', 1;
>         IF NOT EXISTS (SELECT 1 FROM ventas.Clientes WHERE ClienteId = @ClienteId)
>             THROW 50404, N'El cliente no existe.', 1;
>         IF EXISTS (SELECT 1 FROM @Items i
>                    LEFT JOIN ventas.Productos p ON p.ProductoId = i.ProductoId
>                    WHERE p.ProductoId IS NULL)
>             THROW 50404, N'Algún producto no existe.', 1;
>
>         BEGIN TRANSACTION;
>             INSERT INTO ventas.Pedidos (ClienteId, Fecha)
>             VALUES (@ClienteId, CAST(SYSUTCDATETIME() AS date));
>             SET @PedidoId = SCOPE_IDENTITY();
>
>             INSERT INTO ventas.PedidoDetalle (PedidoId, ProductoId, Cantidad, PrecioUnitario)
>             SELECT @PedidoId, i.ProductoId, i.Cantidad, p.Precio
>             FROM   @Items i
>             JOIN   ventas.Productos p ON p.ProductoId = i.ProductoId;
>         COMMIT TRANSACTION;
>     END TRY
>     BEGIN CATCH
>         IF @@TRANCOUNT > 0 ROLLBACK TRANSACTION;
>         THROW;
>     END CATCH
> END
> GO
> ```

> [!success]- Pruebas
> ```sql
> -- Caso feliz
> DECLARE @items ventas.PedidoItem, @id int;
> INSERT INTO @items VALUES (1, 2), (5, 1);
> EXEC ventas.Pedido_Crear @ClienteId = 6, @Items = @items, @PedidoId = @id OUTPUT;
> SELECT @id AS PedidoId;
> SELECT * FROM ventas.PedidoDetalle WHERE PedidoId = @id;
> GO
> -- Atomicidad: 500 unidades
> DECLARE @antes int = (SELECT COUNT(*) FROM ventas.Pedidos);
> DECLARE @items ventas.PedidoItem, @id int;
> INSERT INTO @items VALUES (2, 500);
> BEGIN TRY
>     EXEC ventas.Pedido_Crear @ClienteId = 6, @Items = @items, @PedidoId = @id OUTPUT;
> END TRY
> BEGIN CATCH
>     SELECT ERROR_NUMBER() AS Numero, ERROR_MESSAGE() AS Mensaje;   -- 547
> END CATCH
> SELECT @antes AS Antes, COUNT(*) AS Despues FROM ventas.Pedidos;  -- iguales
> GO
> ```

> [!success]- Paso 6: con y sin XACT_ABORT
> ```sql
> SET XACT_ABORT OFF;
> BEGIN TRAN;
> INSERT INTO ventas.Pedidos (ClienteId, Fecha) VALUES (6, '2025-06-01');
> INSERT INTO ventas.PedidoDetalle VALUES (SCOPE_IDENTITY(), 1, 500, 1200);  -- falla el CHECK, pero sigue
> COMMIT;   -- confirma el pedido sin renglones
> GO
> SELECT p.PedidoId FROM ventas.Pedidos p
> WHERE NOT EXISTS (SELECT 1 FROM ventas.PedidoDetalle d WHERE d.PedidoId = p.PedidoId);  -- el huérfano
> -- Con SET XACT_ABORT ON el error revierte la transacción y aborta el lote: el COMMIT no llega a ejecutarse.
> ```
>
> Borra el huérfano al terminar. Ante un *timeout*, el cliente cancela el lote: con `XACT_ABORT OFF` la transacción queda abierta en la conexión (y vuelve al pool con bloqueos); con `ON` se revierte.

> [!success]- Llamada desde Dapper
> ```csharp
> var items = new DataTable();
> items.Columns.Add("ProductoId", typeof(int));
> items.Columns.Add("Cantidad", typeof(int));
> items.Rows.Add(1, 2);
> items.Rows.Add(5, 1);
>
> var p = new DynamicParameters();
> p.Add("ClienteId", 6);
> p.Add("Items", items.AsTableValuedParameter("ventas.PedidoItem"));
> p.Add("PedidoId", dbType: DbType.Int32, direction: ParameterDirection.Output);
> try
> {
>     await cn.ExecuteAsync("ventas.Pedido_Crear", p, commandType: CommandType.StoredProcedure);
>     Console.WriteLine($"Pedido {p.Get<int>("PedidoId")}");
> }
> catch (SqlException ex) when (ex.Number is 50400 or 50404)
> {
>     Console.WriteLine($"Error de negocio {ex.Number}: {ex.Message}");
> }
> ```

## Notas personales

_Qué me costó, errores que cometí, dudas._
