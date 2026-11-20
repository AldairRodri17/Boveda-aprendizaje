---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: SQL Server
herramienta: SQL Server Developer o Express + SSMS (o VS Code con la extensión MSSQL)
tags: [sql-server, laboratorio]
---
# Lab 3 - CTE y funciones de ventana en Tienda

Parte de: [[06-SQL-Server/02 Intermedio/_Índice Intermedio|Intermedio]]

**Cubre:** [[CTE]] · [[CTE múltiples]] · [[CTE recursivas]] · [[Funciones de ventana]] · [[ROW_NUMBER, RANK y DENSE_RANK]] · [[LAG y LEAD]] · [[Totales acumulados con OVER]] · [[CROSS APPLY y OUTER APPLY]] · [[Vistas]] · [[STRING_AGG y STRING_SPLIT]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Resolver preguntas de análisis con CTE y funciones de ventana sobre la base Tienda ([[Lab 1 - Base de práctica Tienda en SQL Server]]) y publicar una de ellas como vista reutilizable.

## Pasos

Guarda las consultas en `03_analitica.sql`. Ventas = pedidos no cancelados.

1. **Ranking de productos** por importe vendido dentro de su categoría (`DENSE_RANK`) y su participación en la categoría (%).
2. **Recurrencia**: por cada pedido, días desde el pedido anterior del mismo cliente y si es la primera compra.
3. **Mes a mes**: ventas por mes, acumulado (`ROWS`), diferencia contra el mes anterior y promedio móvil de 2 meses.
4. **Último pedido por cliente** de dos formas: `ROW_NUMBER` y `OUTER APPLY` (deben salir los 6 clientes).
5. **Organigrama** con CTE recursiva: nivel, ruta y número de subordinados directos e indirectos de cada empleado.
6. Crea la vista `ventas.vVentasPorMes` (con `CREATE OR ALTER VIEW`) que devuelva mes, importe y acumulado. Consulta la vista con un `ORDER BY`.
7. **Reto**: por cliente, la lista de productos distintos que ha comprado, en orden alfabético y separados por coma.

## Criterios de terminado

- [ ] En la 1, Teclado mecánico es 1.º de Periféricos con 66.0 % y Mouse inalámbrico 2.º con 34.0 %.
- [ ] En la 2, los pedidos 3 y 7 de Ana muestran 36 y 38 días; el 6 de Bruno, 56.
- [ ] En la 3, el acumulado termina en 57750.00 y la diferencia de marzo es +15850.00.
- [ ] La 4 devuelve 6 filas con ambos métodos; Fernando Gil sale con `NULL`.
- [ ] En la 5, Laura Torres tiene 6 subordinados, Mario Vega 3, Iván Cruz 1 y Tania Flores 0.
- [ ] `SELECT * FROM ventas.vVentasPorMes ORDER BY Mes;` devuelve 4 filas.
- [ ] Reto: Ana muestra «Cable HDMI, Monitor 27, Mouse inalámbrico, Teclado mecánico».

## Solución

> [!success]- 1. Ranking dentro de la categoría
> ```sql
> WITH v AS (
>     SELECT pr.Categoria, pr.Nombre, SUM(d.Cantidad * d.PrecioUnitario) AS Importe
>     FROM   ventas.PedidoDetalle d
>     JOIN   ventas.Productos pr ON pr.ProductoId = d.ProductoId
>     JOIN   ventas.Pedidos p    ON p.PedidoId = d.PedidoId
>     WHERE  p.Estado <> 'Cancelado'
>     GROUP  BY pr.Categoria, pr.Nombre
> )
> SELECT Categoria, Nombre, Importe,
>        DENSE_RANK() OVER (PARTITION BY Categoria ORDER BY Importe DESC) AS Lugar,
>        CAST(100.0 * Importe / SUM(Importe) OVER (PARTITION BY Categoria) AS decimal(5,1)) AS PctCategoria
> FROM   v
> ORDER  BY Categoria, Lugar;
> ```
>
> Periféricos: Teclado 3500 (66.0 %), Mouse 1800 (34.0 %).

> [!success]- 2. Recurrencia
> ```sql
> SELECT ClienteId, PedidoId, Fecha,
>        DATEDIFF(day, LAG(Fecha) OVER (PARTITION BY ClienteId ORDER BY Fecha, PedidoId), Fecha) AS DiasDesdeAnterior,
>        CASE WHEN ROW_NUMBER() OVER (PARTITION BY ClienteId ORDER BY Fecha, PedidoId) = 1
>             THEN 1 ELSE 0 END AS PrimeraCompra
> FROM   ventas.Pedidos
> ORDER  BY ClienteId, Fecha;
> ```

> [!success]- 3. Mes a mes
> ```sql
> WITH m AS (
>     SELECT DATEFROMPARTS(YEAR(p.Fecha), MONTH(p.Fecha), 1) AS Mes,
>            SUM(d.Cantidad * d.PrecioUnitario) AS Importe
>     FROM   ventas.Pedidos p
>     JOIN   ventas.PedidoDetalle d ON d.PedidoId = p.PedidoId
>     WHERE  p.Estado <> 'Cancelado'
>     GROUP  BY DATEFROMPARTS(YEAR(p.Fecha), MONTH(p.Fecha), 1)
> )
> SELECT Mes, Importe,
>        SUM(Importe) OVER (ORDER BY Mes ROWS UNBOUNDED PRECEDING)          AS Acumulado,
>        Importe - LAG(Importe) OVER (ORDER BY Mes)                         AS VsAnterior,
>        AVG(Importe) OVER (ORDER BY Mes ROWS BETWEEN 1 PRECEDING AND CURRENT ROW) AS Movil2
> FROM   m;
> ```
>
> 20600 / 26250 / 47750 / 57750; diferencias `NULL`, −14950, +15850, −11500.

> [!success]- 4. Último pedido por cliente
> ```sql
> -- ROW_NUMBER
> WITH r AS (
>     SELECT ClienteId, PedidoId, Fecha,
>            ROW_NUMBER() OVER (PARTITION BY ClienteId ORDER BY Fecha DESC, PedidoId DESC) AS rn
>     FROM   ventas.Pedidos
> )
> SELECT c.Nombre, r.PedidoId, r.Fecha
> FROM   ventas.Clientes c
> LEFT   JOIN r ON r.ClienteId = c.ClienteId AND r.rn = 1;
>
> -- OUTER APPLY
> SELECT c.Nombre, u.PedidoId, u.Fecha
> FROM   ventas.Clientes c
> OUTER  APPLY (SELECT TOP (1) p.PedidoId, p.Fecha
>               FROM   ventas.Pedidos p
>               WHERE  p.ClienteId = c.ClienteId
>               ORDER  BY p.Fecha DESC, p.PedidoId DESC) AS u;
> ```

> [!success]- 5. Organigrama con subordinados
> ```sql
> WITH Org AS (
>     SELECT EmpleadoId, Nombre, JefeId, 0 AS Nivel, CAST(Nombre AS nvarchar(400)) AS Ruta
>     FROM   rh.Empleados WHERE JefeId IS NULL
>     UNION ALL
>     SELECT e.EmpleadoId, e.Nombre, e.JefeId, o.Nivel + 1,
>            CAST(o.Ruta + N' > ' + e.Nombre AS nvarchar(400))
>     FROM   rh.Empleados e JOIN Org o ON e.JefeId = o.EmpleadoId
> )
> SELECT o.Nivel, o.Ruta,
>        (SELECT COUNT(*) FROM Org x
>         WHERE x.Ruta LIKE o.Ruta + N' > %') AS Subordinados
> FROM   Org o
> ORDER  BY o.Ruta;
> ```
>
> Contar descendientes comparando rutas es didáctico; en tablas grandes conviene `hierarchyid` o una CTE recursiva por empleado.

> [!success]- 6. Vista
> ```sql
> CREATE OR ALTER VIEW ventas.vVentasPorMes
> AS
> WITH m AS (
>     SELECT DATEFROMPARTS(YEAR(p.Fecha), MONTH(p.Fecha), 1) AS Mes,
>            SUM(d.Cantidad * d.PrecioUnitario) AS Importe
>     FROM   ventas.Pedidos p
>     JOIN   ventas.PedidoDetalle d ON d.PedidoId = p.PedidoId
>     WHERE  p.Estado <> 'Cancelado'
>     GROUP  BY DATEFROMPARTS(YEAR(p.Fecha), MONTH(p.Fecha), 1)
> )
> SELECT Mes, Importe,
>        SUM(Importe) OVER (ORDER BY Mes ROWS UNBOUNDED PRECEDING) AS Acumulado
> FROM   m;
> GO
> SELECT * FROM ventas.vVentasPorMes ORDER BY Mes;
> ```
>
> El `ORDER BY` va al consultar, no dentro de la vista.

> [!success]- 7. Reto: productos por cliente
> ```sql
> WITH cp AS (
>     SELECT DISTINCT p.ClienteId, pr.Nombre
>     FROM   ventas.Pedidos p
>     JOIN   ventas.PedidoDetalle d ON d.PedidoId = p.PedidoId
>     JOIN   ventas.Productos pr    ON pr.ProductoId = d.ProductoId
> )
> SELECT c.Nombre, STRING_AGG(cp.Nombre, N', ') WITHIN GROUP (ORDER BY cp.Nombre) AS Productos
> FROM   ventas.Clientes c
> JOIN   cp ON cp.ClienteId = c.ClienteId
> GROUP  BY c.Nombre;
> ```
>
> `STRING_AGG` no admite `DISTINCT`, por eso se quitan duplicados antes en la CTE.

## Notas personales

_Qué me costó, errores que cometí, dudas._
