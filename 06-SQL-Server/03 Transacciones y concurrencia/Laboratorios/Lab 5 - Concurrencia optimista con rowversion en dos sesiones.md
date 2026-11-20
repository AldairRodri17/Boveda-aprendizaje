---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: SQL Server
herramienta: SQL Server Developer o Express + SSMS (dos ventanas de consulta)
tags: [sql-server, laboratorio]
---
# Lab 5 - Concurrencia optimista con rowversion en dos sesiones

Parte de: [[06-SQL-Server/03 Transacciones y concurrencia/_Índice Transacciones y concurrencia|Transacciones y concurrencia]]

**Cubre:** [[Concurrencia optimista]] · [[TRY CATCH y THROW]] · [[OUTPUT]] · [[Stored procedures]] · [[READ COMMITTED (nivel por defecto)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Simular dos usuarios que editan el mismo pedido y comprobar que la concurrencia optimista con `rowversion` detecta el conflicto y responde con el error **50409**, en vez de que «gane el último». Usa la base Tienda ([[Lab 1 - Base de práctica Tienda en SQL Server]]); `ventas.Pedidos` ya tiene la columna `RowVer`.

## Pasos

1. Crea `ventas.Pedido_CambiarEstado @PedidoId int, @Estado varchar(20), @RowVer binary(8)` que:
   - actualice solo si `RowVer = @RowVer` y devuelva el nuevo `RowVer` con `OUTPUT`;
   - si no afectó filas, lance `50409` si el pedido existe o `50404` si no.
2. Abre **dos ventanas** en SSMS (sesión A y sesión B). En ambas lee el pedido 7: `SELECT PedidoId, Estado, RowVer FROM ventas.Pedidos WHERE PedidoId = 7;` y copia el `RowVer`.
3. En B, cambia el estado a `Enviado` con el `RowVer` leído. Debe funcionar y devolver un `RowVer` nuevo.
4. En A, intenta cambiar el estado a `Cancelado` con el `RowVer` **viejo**. Debe fallar con 50409.
5. En A, vuelve a leer, toma el `RowVer` actual y reintenta: ahora sí funciona.
6. Prueba con `PedidoId = 999` para ver el 50404.
7. Compara con un `UPDATE` sin control (`WHERE PedidoId = 7` solamente) desde las dos sesiones: el segundo pisa al primero sin aviso.
8. (Opcional) Desde C# con Dapper, envía el `RowVer` como `byte[]` y traduce `SqlException.Number == 50409` a HTTP 409.

## Criterios de terminado

- [ ] El paso 3 devuelve una fila con un `RowVer` distinto al leído.
- [ ] El paso 4 falla con `Msg 50409` y el estado sigue siendo `Enviado`.
- [ ] El paso 5 deja el pedido en `Cancelado`.
- [ ] El paso 6 falla con `Msg 50404`.
- [ ] Puedo explicar por qué con `SET NOCOUNT ON` la app no puede confiar en el número de filas que devuelve `ExecuteAsync` y por eso la verificación vive en el procedimiento.
- [ ] Al final, regreso el pedido 7 a `Pendiente`.

## Solución

> [!success]- Procedimiento
> ```sql
> CREATE OR ALTER PROCEDURE ventas.Pedido_CambiarEstado
>     @PedidoId int,
>     @Estado   varchar(20),
>     @RowVer   binary(8)
> AS
> BEGIN
>     SET NOCOUNT ON;
>     SET XACT_ABORT ON;
>
>     UPDATE ventas.Pedidos
>     SET    Estado = @Estado
>     OUTPUT inserted.PedidoId, inserted.Estado, inserted.RowVer
>     WHERE  PedidoId = @PedidoId AND RowVer = @RowVer;
>
>     IF @@ROWCOUNT = 0
>     BEGIN
>         IF EXISTS (SELECT 1 FROM ventas.Pedidos WHERE PedidoId = @PedidoId)
>             THROW 50409, N'El pedido cambió desde que lo leíste. Recarga e intenta de nuevo.', 1;
>         ELSE
>             THROW 50404, N'El pedido no existe.', 1;
>     END
> END
> GO
> ```
>
> Si `ventas.Pedidos` tuviera triggers, el `OUTPUT` necesitaría `INTO @tabla` (error 334).

> [!success]- Sesiones A y B
> ```sql
> -- A y B
> SELECT PedidoId, Estado, RowVer FROM ventas.Pedidos WHERE PedidoId = 7;   -- p. ej. 0x00000000000007D3
>
> -- B
> EXEC ventas.Pedido_CambiarEstado @PedidoId = 7, @Estado = 'Enviado', @RowVer = 0x00000000000007D3;  -- OK
>
> -- A (versión vieja)
> EXEC ventas.Pedido_CambiarEstado @PedidoId = 7, @Estado = 'Cancelado', @RowVer = 0x00000000000007D3;
> -- Msg 50409, Level 16, State 1: El pedido cambió desde que lo leíste...
> ```
>
> El valor de `RowVer` será distinto en tu máquina: usa el que te devuelva el `SELECT`.

> [!success]- Dapper
> ```csharp
> try
> {
>     var fila = await cn.QuerySingleAsync<(int PedidoId, string Estado, byte[] RowVer)>(
>         "ventas.Pedido_CambiarEstado",
>         new { PedidoId = 7, Estado = "Cancelado", RowVer = rowVerLeido },
>         commandType: CommandType.StoredProcedure);
>     return Results.Ok(new { fila.Estado, RowVer = Convert.ToBase64String(fila.RowVer) });
> }
> catch (SqlException ex) when (ex.Number == 50409)
> {
>     return Results.Conflict(new ProblemDetails { Title = ex.Message });
> }
> catch (SqlException ex) when (ex.Number == 50404)
> {
>     return Results.NotFound();
> }
> ```

## Notas personales

_Qué me costó, errores que cometí, dudas._
