---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: SQL Server
tags: [sql-server]
---
# GROUP BY y HAVING

Parte de: [[06-SQL-Server/01 Básico/_Índice Básico|Básico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`GROUP BY` agrupa las filas que comparten los mismos valores en una o más columnas para calcular [[Funciones de agregación]] por grupo: ventas por categoría, pedidos por mes, clientes por ciudad. `HAVING` filtra **los grupos** ya formados según el resultado de sus agregados ("solo las categorías con más de 1,000 de venta"). Juntos son el corazón de cualquier reporte.

### Cómo funciona
El orden lógico de evaluación explica casi todas las dudas ([[SELECT]]):

```mermaid
flowchart LR
  F[FROM / JOIN] --> W[WHERE filtra filas] --> G[GROUP BY forma grupos] --> H[HAVING filtra grupos] --> S[SELECT calcula columnas] --> O[ORDER BY]
```

- **`WHERE` actúa antes de agrupar** y sobre filas individuales; no puede usar agregados (`WHERE SUM(x) > 10` da error).
- **`HAVING` actúa después** y sobre grupos; normalmente usa agregados. Un filtro que no depende de agregados (`Estado <> 'Cancelado'`) debe ir en `WHERE`: reduce las filas antes de agrupar y es más barato.
- **Regla de oro**: con `GROUP BY`, cada columna del `SELECT` debe estar en el `GROUP BY` **o** dentro de un agregado. `SELECT Categoria, Nombre, SUM(Precio) ... GROUP BY Categoria` falla (error 8120) porque un grupo de categoría tiene varios nombres y SQL Server no elige uno al azar.
- Puedes agrupar por **expresiones** (`GROUP BY YEAR(Fecha), MONTH(Fecha)`), y la misma expresión debe aparecer en el `SELECT`. Los alias del `SELECT` no se pueden usar en `GROUP BY` ni `HAVING` (se evalúan después), pero sí en `ORDER BY`.
- Los `NULL` forman **un solo grupo**.

**Varios niveles de totales en una consulta**:

- `GROUP BY ROLLUP (Anio, Mes)`: detalle por año-mes, subtotal por año y gran total.
- `GROUP BY CUBE (a, b)`: todas las combinaciones de subtotales.
- `GROUP BY GROUPING SETS ((Categoria), (Ciudad), ())`: exactamente los agrupamientos que enumeras.
- `GROUPING(col)` devuelve 1 cuando esa columna está "agregada" en la fila (subtotal), útil para distinguir el `NULL` de subtotal de un `NULL` real.

### Ejemplo
```sql
-- Ventas por categoría y mes, solo grupos relevantes
SELECT pr.Categoria,
       DATEFROMPARTS(YEAR(p.Fecha), MONTH(p.Fecha), 1) AS Mes,
       COUNT(DISTINCT p.PedidoId)          AS Pedidos,
       SUM(d.Cantidad)                     AS Unidades,
       SUM(d.Cantidad * d.PrecioUnitario)  AS Importe
FROM   ventas.Pedidos p
JOIN   ventas.PedidoDetalle d ON d.PedidoId = p.PedidoId
JOIN   ventas.Productos pr    ON pr.ProductoId = d.ProductoId
WHERE  p.Estado <> 'Cancelado'                        -- filtro de filas: WHERE
GROUP  BY pr.Categoria, DATEFROMPARTS(YEAR(p.Fecha), MONTH(p.Fecha), 1)
HAVING SUM(d.Cantidad * d.PrecioUnitario) >= 1000     -- filtro de grupos: HAVING
ORDER  BY Mes, Importe DESC;

-- Subtotales por año y gran total con ROLLUP
SELECT CASE WHEN GROUPING(YEAR(p.Fecha)) = 1 THEN N'Total'
            ELSE CAST(YEAR(p.Fecha) AS nvarchar(4)) END        AS Anio,
       CASE WHEN GROUPING(pr.Categoria) = 1 THEN N'(todas)'
            ELSE pr.Categoria END                              AS Categoria,
       SUM(d.Cantidad * d.PrecioUnitario)                      AS Importe
FROM   ventas.Pedidos p
JOIN   ventas.PedidoDetalle d ON d.PedidoId = p.PedidoId
JOIN   ventas.Productos pr    ON pr.ProductoId = d.ProductoId
WHERE  p.Estado <> 'Cancelado'
GROUP  BY ROLLUP (YEAR(p.Fecha), pr.Categoria);

-- Agrupar NULL con una etiqueta
SELECT COALESCE(Ciudad, N'(sin ciudad)') AS Ciudad, COUNT(*) AS Clientes
FROM   ventas.Clientes
GROUP  BY COALESCE(Ciudad, N'(sin ciudad)');
```

En Wee.Reporteria, un `ROLLUP` evita calcular subtotales en C# antes de escribir el Excel: Dapper trae las filas y el código solo aplica formato a las que tienen la marca de subtotal (devuelve `GROUPING(...)` como columna `EsSubtotal`).

### Cuándo usarlo y cuándo no
- **`GROUP BY`** cuando quieres una fila por grupo.
- **Funciones de ventana** cuando quieres el detalle **y** el agregado juntos (cada pedido con el total del cliente); `GROUP BY` colapsa filas, `OVER` no. Ver [[Funciones de ventana]].
- **`HAVING`** solo para condiciones sobre agregados; lo demás en `WHERE`.
- Para obtener "la fila completa con el máximo por grupo" (el último pedido de cada cliente con todos sus datos), `GROUP BY` no basta: usa `ROW_NUMBER()` (ver [[ROW_NUMBER, RANK y DENSE_RANK]]).

### Errores comunes
1. **Columna no agregada ni agrupada** en el `SELECT`: error 8120. Decide si va en el `GROUP BY` o dentro de un `MAX`/`MIN`.
2. **Filtros de fila en `HAVING`**: funcionan, pero agrupan de más; muévelos a `WHERE`.
3. **Usar alias del `SELECT` en `HAVING`**: no existen todavía; repite la expresión.
4. **Agrupar por una columna de alta cardinalidad sin querer** (por ejemplo la fecha con hora en vez del mes): salen miles de grupos de una fila.
5. **Sumar tras un JOIN con dos tablas hijas**: importes multiplicados (fan-out, ver [[JOINs]]).

### Para profundizar
- [SELECT - GROUP BY (Transact-SQL)](https://learn.microsoft.com/sql/t-sql/queries/select-group-by-transact-sql)
- [SELECT - HAVING (Transact-SQL)](https://learn.microsoft.com/sql/t-sql/queries/select-having-transact-sql)
- [GROUPING (Transact-SQL)](https://learn.microsoft.com/sql/t-sql/functions/grouping-transact-sql)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Con la base Tienda ([[Lab 1 - Base de práctica Tienda en SQL Server]]): por categoría, unidades vendidas e importe (sin pedidos cancelados), solo las categorías con más de 1,000 de venta.
> ```sql
> SELECT pr.Categoria,
>        SUM(d.Cantidad)                    AS Unidades,
>        SUM(d.Cantidad * d.PrecioUnitario) AS Importe
> FROM   ventas.PedidoDetalle d
> JOIN   ventas.Productos pr ON pr.ProductoId = d.ProductoId
> JOIN   ventas.Pedidos  p  ON p.PedidoId   = d.PedidoId
> WHERE  p.Estado <> 'Cancelado'
> GROUP  BY pr.Categoria
> HAVING SUM(d.Cantidad * d.PrecioUnitario) > 1000
> ORDER  BY Importe DESC;
> ```
> Computadoras 2 / 36500, Monitores 3 / 15200, Periféricos 7 / 5300 (Accesorios, con 750, queda fuera).

> [!question]- Ventas por mes (sin cancelados): número de pedidos e importe.
> ```sql
> SELECT YEAR(p.Fecha) AS Anio, MONTH(p.Fecha) AS Mes,
>        COUNT(DISTINCT p.PedidoId)         AS Pedidos,
>        SUM(d.Cantidad * d.PrecioUnitario) AS Importe
> FROM   ventas.Pedidos p
> JOIN   ventas.PedidoDetalle d ON d.PedidoId = p.PedidoId
> WHERE  p.Estado <> 'Cancelado'
> GROUP  BY YEAR(p.Fecha), MONTH(p.Fecha)
> ORDER  BY Anio, Mes;
> ```
> Ene 2 / 20600, Feb 1 / 5650, Mar 3 / 21500, Abr 1 / 10000. `COUNT(DISTINCT)` porque el join repite el pedido por cada renglón.

> [!question]- Clientes con 2 o más pedidos (de cualquier estado).
> ```sql
> SELECT c.Nombre, COUNT(*) AS Pedidos
> FROM   ventas.Clientes c
> JOIN   ventas.Pedidos p ON p.ClienteId = c.ClienteId
> GROUP  BY c.ClienteId, c.Nombre
> HAVING COUNT(*) >= 2;
> ```
> Ana López (3) y Bruno Díaz (2).

> [!question]- Ventas por ciudad del cliente (sin cancelados); los clientes sin ciudad en un grupo `(sin ciudad)`.
> ```sql
> SELECT COALESCE(c.Ciudad, N'(sin ciudad)') AS Ciudad,
>        SUM(d.Cantidad * d.PrecioUnitario) AS Importe
> FROM   ventas.Clientes c
> JOIN   ventas.Pedidos p ON p.ClienteId = c.ClienteId
> JOIN   ventas.PedidoDetalle d ON d.PedidoId = p.PedidoId
> WHERE  p.Estado <> 'Cancelado'
> GROUP  BY c.Ciudad
> ORDER  BY Importe DESC;
> ```
> Monterrey 21100, CDMX 18450, (sin ciudad) 10000, Guadalajara 8200.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre `WHERE` y `HAVING`?
> `WHERE` filtra filas **antes** de agrupar; `HAVING` filtra **grupos** después de agrupar y puede usar agregados (`HAVING SUM(x) > 1000`). Filtra en `WHERE` todo lo que puedas: es más barato.

> [!question]- ¿Por qué falla `SELECT Categoria, Nombre, SUM(Precio) FROM ventas.Productos GROUP BY Categoria`?
> Error 8120: `Nombre` no está en el `GROUP BY` ni dentro de un agregado. Cada columna del `SELECT` debe ser parte de la agrupación o estar agregada.

> [!question]- ¿Qué hacen `ROLLUP` y `GROUPING SETS`?
> Calculan varios niveles de agrupación en una consulta: `GROUP BY ROLLUP (Categoria, Nombre)` agrega subtotales por categoría y un total general. `GROUPING(col)` te dice si la fila es un subtotal.

## Práctica

- [[Lab 2 - Consultas de agregación y JOINs en Tienda]]

