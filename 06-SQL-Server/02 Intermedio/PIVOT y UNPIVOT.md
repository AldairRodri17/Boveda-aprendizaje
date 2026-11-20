---
proyecto: Conocimiento
tipo: concepto
nivel: avanzado
progreso: pendiente
estado: pendiente
area: SQL Server
tags: [sql-server]
---
# PIVOT y UNPIVOT

Parte de: [[06-SQL-Server/02 Intermedio/_Índice Intermedio|Intermedio]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`PIVOT` convierte **valores de filas en columnas**: de una lista "Categoría, Mes, Importe" obtienes una tabla con una fila por categoría y una columna por mes (Ene, Feb, Mar, Abr), como una tabla dinámica de Excel. `UNPIVOT` hace lo contrario: toma varias columnas y las convierte en filas con pares nombre-valor. Son operadores de T-SQL que se escriben en la cláusula `FROM`.

### Cómo funciona
`PIVOT` hace tres cosas a la vez, y entenderlas evita casi todos sus errores:

1. **Agrupa** por todas las columnas del origen que **no** mencionas en el `PIVOT` (agrupación implícita).
2. **Reparte** los valores de la columna pivote (`Mes`) en las columnas que listas en `IN ([1],[2],[3],[4])`.
3. **Agrega** la columna de valor (`SUM(Importe)`) para cada combinación.

Como la agrupación es implícita, **si el origen trae columnas de más** (por ejemplo `PedidoId` o `Fecha` completa), `PIVOT` agrupa también por ellas y devuelve **más filas de las esperadas**, con muchos `NULL`. Por eso el origen se prepara siempre en una tabla derivada o [[CTE]] que contenga **solo** tres tipos de columna: las de agrupación, la pivote y la de valor.

La otra limitación importante: **las columnas resultado deben escribirse a mano** en `IN (...)`. No puedes decir "una columna por cada mes que exista". Si los valores cambian, necesitas construir la consulta con [[SQL dinámico y sp_executesql]].

```mermaid
flowchart LR
    A["Filas: Categoria, Mes, Importe"] --> B["Tabla derivada con solo esas 3 columnas"]
    B --> C["PIVOT SUM(Importe) FOR Mes IN ([1],[2],[3],[4])"]
    C --> D["Una fila por Categoria, una columna por mes"]
    D -->|"UNPIVOT"| A
```

`UNPIVOT` toma columnas listadas y produce dos columnas: una con el nombre de la columna original y otra con su valor. **Descarta las filas cuyo valor es `NULL`**, algo a tener en cuenta si necesitas conservar los ceros o vacíos.

### Ejemplo
Con la base de [[Lab 1 - Base de práctica Tienda en SQL Server]]: importe por categoría y mes (enero a abril de 2025).

```sql
SELECT Categoria,
       ISNULL([1], 0) AS Ene, ISNULL([2], 0) AS Feb,
       ISNULL([3], 0) AS Mar, ISNULL([4], 0) AS Abr
FROM (
    SELECT pr.Categoria, MONTH(p.Fecha) AS Mes, d.Cantidad * d.PrecioUnitario AS Importe
    FROM   ventas.Pedidos p
    JOIN   ventas.PedidoDetalle d ON d.PedidoId   = p.PedidoId
    JOIN   ventas.Productos pr    ON pr.ProductoId = d.ProductoId
    WHERE  p.Estado <> 'Cancelado'
) AS src
PIVOT (SUM(Importe) FOR Mes IN ([1], [2], [3], [4])) AS pv
ORDER BY Categoria;

-- Lo mismo con agregación condicional (sin PIVOT)
SELECT pr.Categoria,
       SUM(CASE WHEN MONTH(p.Fecha) = 1 THEN d.Cantidad * d.PrecioUnitario ELSE 0 END) AS Ene,
       SUM(CASE WHEN MONTH(p.Fecha) = 2 THEN d.Cantidad * d.PrecioUnitario ELSE 0 END) AS Feb,
       SUM(CASE WHEN MONTH(p.Fecha) = 3 THEN d.Cantidad * d.PrecioUnitario ELSE 0 END) AS Mar,
       SUM(CASE WHEN MONTH(p.Fecha) = 4 THEN d.Cantidad * d.PrecioUnitario ELSE 0 END) AS Abr
FROM   ventas.Pedidos p
JOIN   ventas.PedidoDetalle d ON d.PedidoId   = p.PedidoId
JOIN   ventas.Productos pr    ON pr.ProductoId = d.ProductoId
WHERE  p.Estado <> 'Cancelado'
GROUP  BY pr.Categoria;

-- De columnas a filas
SELECT Categoria, Mes, Importe
FROM   #Pivote
UNPIVOT (Importe FOR Mes IN (Ene, Feb, Mar, Abr)) AS u;
```

La agregación condicional es más larga pero más flexible: puedes pivotear varias medidas a la vez (importe y cantidad), usar condiciones distintas por columna y el `GROUP BY` es explícito. Muchos equipos la prefieren por claridad. Con Dapper, una consulta pivoteada se mapea bien a un DTO con propiedades fijas (`Ene`, `Feb`...); si las columnas son dinámicas, lee el resultado como `IEnumerable<dynamic>` o, mejor, devuelve filas sin pivotear y arma la matriz en el cliente.

### Cuándo usarlo y cuándo no
- **Úsalo** para reportes con columnas conocidas de antemano (meses, trimestres, estados fijos).
- **Prefiere devolver filas** y pivotear en la capa de presentación (React, Excel en Wee.Reporteria, Power BI) cuando las columnas cambian o cuando el consumidor ya sabe pivotear: la consulta queda simple y estable.
- **SQL dinámico** solo si de verdad necesitas columnas variables en SQL, con nombres escapados con `QUOTENAME`.
- `UNPIVOT` es útil para normalizar datos importados de hojas de cálculo; `CROSS APPLY (VALUES ...)` es una alternativa que además conserva los `NULL`.

### Errores comunes
- **Más filas de las esperadas**: el origen trae columnas extra que se vuelven agrupación implícita; usa una tabla derivada con solo lo necesario.
- **`NULL` en vez de 0**: `PIVOT` deja `NULL` donde no hubo datos; envuelve con `ISNULL`.
- **Olvidar los corchetes** en valores numéricos: `IN (1, 2)` es error; debe ser `IN ([1], [2])`.
- **`UNPIVOT` pierde filas**: descarta los `NULL`; usa `CROSS APPLY (VALUES ...)` si debes conservarlos.
- **Tipos distintos en `UNPIVOT`**: todas las columnas que desarmas deben tener el mismo tipo y longitud.

### Para profundizar
- [Using PIVOT and UNPIVOT](https://learn.microsoft.com/en-us/sql/t-sql/queries/from-using-pivot-and-unpivot)
- [CASE (Transact-SQL)](https://learn.microsoft.com/en-us/sql/t-sql/language-elements/case-transact-sql)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Con la base Tienda ([[Lab 1 - Base de práctica Tienda en SQL Server]]): tabla de importe (sin cancelados) por categoría y mes (enero a abril) con `PIVOT`.
> ```sql
> SELECT Categoria, ISNULL([1],0) AS Ene, ISNULL([2],0) AS Feb, ISNULL([3],0) AS Mar, ISNULL([4],0) AS Abr
> FROM (
>     SELECT pr.Categoria, MONTH(p.Fecha) AS Mes, d.Cantidad * d.PrecioUnitario AS Importe
>     FROM   ventas.Pedidos p
>     JOIN   ventas.PedidoDetalle d ON d.PedidoId = p.PedidoId
>     JOIN   ventas.Productos pr    ON pr.ProductoId = d.ProductoId
>     WHERE  p.Estado <> 'Cancelado'
> ) AS src
> PIVOT (SUM(Importe) FOR Mes IN ([1],[2],[3],[4])) AS pv
> ORDER BY Categoria;
> ```
> Accesorios 0/450/300/0; Computadoras 18500/0/18000/0; Monitores 0/5200/0/10000; Periféricos 2100/0/3200/0.

> [!question]- El mismo resultado sin `PIVOT` (agregación condicional).
> ```sql
> SELECT pr.Categoria,
>        SUM(CASE WHEN MONTH(p.Fecha) = 1 THEN d.Cantidad * d.PrecioUnitario ELSE 0 END) AS Ene,
>        SUM(CASE WHEN MONTH(p.Fecha) = 2 THEN d.Cantidad * d.PrecioUnitario ELSE 0 END) AS Feb,
>        SUM(CASE WHEN MONTH(p.Fecha) = 3 THEN d.Cantidad * d.PrecioUnitario ELSE 0 END) AS Mar,
>        SUM(CASE WHEN MONTH(p.Fecha) = 4 THEN d.Cantidad * d.PrecioUnitario ELSE 0 END) AS Abr
> FROM   ventas.Pedidos p
> JOIN   ventas.PedidoDetalle d ON d.PedidoId = p.PedidoId
> JOIN   ventas.Productos pr ON pr.ProductoId = d.ProductoId
> WHERE  p.Estado <> 'Cancelado'
> GROUP  BY pr.Categoria;
> ```
> Más portable (funciona igual en PostgreSQL) y permite varias métricas a la vez.

> [!question]- Si guardaste el resultado anterior en `#Pivote(Categoria, Ene, Feb, Mar, Abr)`, regrésalo a filas con `UNPIVOT`.
> ```sql
> SELECT Categoria, Mes, Importe
> FROM   #Pivote
> UNPIVOT (Importe FOR Mes IN (Ene, Feb, Mar, Abr)) AS u;
> ```
> 16 filas. `UNPIVOT` descarta los `NULL` (aquí no hay porque usamos 0). Alternativa flexible: `CROSS APPLY (VALUES ('Ene', Ene), ('Feb', Feb), ...)`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hacen `PIVOT` y `UNPIVOT`?
> `PIVOT` convierte valores de filas en columnas (meses como columnas); `UNPIVOT` hace lo contrario (columnas `Ene`, `Feb` → filas `Mes`, `Importe`).

> [!question]- ¿Qué limitación tiene `PIVOT` con las columnas?
> Los valores que se vuelven columnas deben escribirse fijos en `IN ([1],[2],...)`. Si cambian (meses dinámicos), necesitas SQL dinámico o pivotar en el reporte/cliente.

> [!question]- Error común: tu `PIVOT` devuelve más filas de las esperadas. ¿Por qué?
> Todas las columnas de la consulta fuente que no son la de pivote ni la agregada se usan como agrupación implícita. Si dejas `PedidoId` en la fuente, sale una fila por pedido. Selecciona solo las columnas necesarias en la subconsulta.

