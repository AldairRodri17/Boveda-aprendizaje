---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: SQL Server
herramienta: SQL Server Developer o Express + SSMS (o VS Code con la extensión MSSQL)
tags: [sql-server, laboratorio]
---
# Lab 2 - Consultas de agregación y JOINs en Tienda

Parte de: [[06-SQL-Server/01 Básico/_Índice Básico|Básico]]

**Cubre:** [[SELECT]] · [[WHERE, LIKE, IN y BETWEEN]] · [[ORDER BY, TOP y OFFSET FETCH]] · [[NULL y lógica de tres valores]] · [[CASE]] · [[Funciones de agregación]] · [[GROUP BY y HAVING]] · [[JOINs]] · [[INNER JOIN]] · [[LEFT y RIGHT JOIN]] · [[UNION, UNION ALL, INTERSECT y EXCEPT]] · [[Funciones de fecha]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Construir un pequeño **reporte de ventas** sobre la base Tienda ([[Lab 1 - Base de práctica Tienda en SQL Server]]) combinando joins, agregación, `CASE` y manejo de `NULL`, y validar cada consulta contra resultados conocidos.

## Pasos

Guarda todas las consultas en `02_reporte.sql`. Considera «venta» todo pedido que **no** esté cancelado.

1. **Resumen por cliente**: nombre, ciudad (o `(sin ciudad)`), número de pedidos, importe total y ticket promedio. Deben salir **los 6 clientes**, con 0 si no tienen ventas. Ordena por importe descendente.
2. **Clasificación**: agrega una columna `Segmento` con `CASE`: «Oro» (≥ 15,000), «Plata» (≥ 5,000), «Bronce» (> 0) y «Sin compras».
3. **Ventas por categoría y mes**: categoría, año, mes, unidades e importe, solo grupos con importe ≥ 1,000.
4. **Productos sin ventas y clientes sin pedidos** en una sola lista con columnas `Tipo` y `Nombre` (usa `UNION ALL`).
5. **Top 3 renglones** de mayor importe con nombre de producto, cliente y fecha del pedido.
6. **Reto**: el día de la semana con más pedidos (todos los estados), con su número; resuelve el empate si lo hay mostrando todos los empatados.

## Criterios de terminado

- [ ] La consulta 1 devuelve 6 filas: Bruno 2 pedidos / 21100.00, Diego 1 / 18450.00, Elena 1 / 10000.00, Ana 3 / 8200.00, Carla 0 / 0 y Fernando 0 / 0.
- [ ] En la 2, Bruno y Diego son «Oro», Elena y Ana «Plata», Carla y Fernando «Sin compras».
- [ ] La 3 devuelve 6 filas, y la de Computadoras de enero tiene 18500.00.
- [ ] La 4 devuelve exactamente 2 filas: Webcam HD (producto) y Fernando Gil (cliente).
- [ ] La 5 devuelve Laptop 14 de Bruno (18500), Laptop 14 de Diego (18000) y Monitor 27 de Elena (10000).
- [ ] Ninguna consulta usa `SELECT *` ni `DISTINCT` para tapar duplicados de un join.

## Solución

> [!success]- 1 y 2. Resumen y segmento por cliente
> ```sql
> WITH Ventas AS (
>     SELECT p.ClienteId, p.PedidoId, SUM(d.Cantidad * d.PrecioUnitario) AS Total
>     FROM   ventas.Pedidos p
>     JOIN   ventas.PedidoDetalle d ON d.PedidoId = p.PedidoId
>     WHERE  p.Estado <> 'Cancelado'
>     GROUP  BY p.ClienteId, p.PedidoId
> )
> SELECT c.Nombre,
>        COALESCE(c.Ciudad, N'(sin ciudad)') AS Ciudad,
>        COUNT(v.PedidoId)                   AS Pedidos,
>        ISNULL(SUM(v.Total), 0)             AS Importe,
>        AVG(v.Total)                        AS TicketPromedio,
>        CASE WHEN SUM(v.Total) >= 15000 THEN N'Oro'
>             WHEN SUM(v.Total) >= 5000  THEN N'Plata'
>             WHEN SUM(v.Total) > 0      THEN N'Bronce'
>             ELSE N'Sin compras' END        AS Segmento
> FROM   ventas.Clientes c
> LEFT   JOIN Ventas v ON v.ClienteId = c.ClienteId
> GROUP  BY c.ClienteId, c.Nombre, c.Ciudad
> ORDER  BY Importe DESC;
> ```
>
> Agregar primero por pedido (CTE) evita que el ticket promedio se calcule por renglón. Ticket de Ana: 8200 / 3 = 2733.33.

> [!success]- 3. Categoría y mes
> ```sql
> SELECT pr.Categoria, YEAR(p.Fecha) AS Anio, MONTH(p.Fecha) AS Mes,
>        SUM(d.Cantidad) AS Unidades, SUM(d.Cantidad * d.PrecioUnitario) AS Importe
> FROM   ventas.Pedidos p
> JOIN   ventas.PedidoDetalle d ON d.PedidoId = p.PedidoId
> JOIN   ventas.Productos pr    ON pr.ProductoId = d.ProductoId
> WHERE  p.Estado <> 'Cancelado'
> GROUP  BY pr.Categoria, YEAR(p.Fecha), MONTH(p.Fecha)
> HAVING SUM(d.Cantidad * d.PrecioUnitario) >= 1000
> ORDER  BY Anio, Mes, Importe DESC;
> ```
>
> Ene: Computadoras 18500, Periféricos 2100. Feb: Monitores 5200. Mar: Computadoras 18000, Periféricos 3200. Abr: Monitores 10000.

> [!success]- 4. Huérfanos
> ```sql
> SELECT N'Producto sin ventas' AS Tipo, pr.Nombre
> FROM   ventas.Productos pr
> WHERE  NOT EXISTS (SELECT 1 FROM ventas.PedidoDetalle d WHERE d.ProductoId = pr.ProductoId)
> UNION ALL
> SELECT N'Cliente sin pedidos', c.Nombre
> FROM   ventas.Clientes c
> WHERE  NOT EXISTS (SELECT 1 FROM ventas.Pedidos p WHERE p.ClienteId = c.ClienteId);
> ```

> [!success]- 5. Top 3 renglones
> ```sql
> SELECT TOP (3) pr.Nombre AS Producto, c.Nombre AS Cliente, p.Fecha,
>        d.Cantidad * d.PrecioUnitario AS Importe
> FROM   ventas.PedidoDetalle d
> JOIN   ventas.Pedidos p    ON p.PedidoId = d.PedidoId
> JOIN   ventas.Clientes c   ON c.ClienteId = p.ClienteId
> JOIN   ventas.Productos pr ON pr.ProductoId = d.ProductoId
> WHERE  p.Estado <> 'Cancelado'
> ORDER  BY Importe DESC;
> ```

> [!success]- 6. Reto: día de la semana
> ```sql
> SELECT TOP (1) WITH TIES DATENAME(weekday, Fecha) AS Dia, COUNT(*) AS Pedidos
> FROM   ventas.Pedidos
> GROUP  BY DATENAME(weekday, Fecha)
> ORDER  BY COUNT(*) DESC;
> ```
>
> El nombre del día depende de `SET LANGUAGE`. Compara tu resultado con `DATEPART(weekday, Fecha)` y recuerda que este depende de `SET DATEFIRST`.

## Notas personales

_Qué me costó, errores que cometí, dudas._
