---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: SQL Server
tags: [sql-server]
---
# Funciones de agregación

Parte de: [[06-SQL-Server/01 Básico/_Índice Básico|Básico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Las **funciones de agregación** reducen un conjunto de filas a un solo valor: `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`, además de `COUNT_BIG`, `STRING_AGG`, `STDEV`, `VAR` y otras. Sin `GROUP BY` resumen toda la tabla en una fila; con `GROUP BY` producen un valor por grupo (ver [[GROUP BY y HAVING]]). Son la base de cualquier reporte: totales, promedios, conteos, extremos.

### Cómo funciona
**Manejo de `NULL`** (lo más preguntado):

- `COUNT(*)` cuenta **filas**, sin importar el contenido: es el único que "ve" los `NULL`.
- `COUNT(col)`, `SUM`, `AVG`, `MIN`, `MAX` **ignoran los `NULL`**. Así, `AVG(Descuento)` promedia solo las filas con descuento, no todas (si quieres tratarlos como 0, `AVG(COALESCE(Descuento, 0))`).
- `COUNT(DISTINCT col)` cuenta valores distintos no nulos.

**Sobre cero filas**: `COUNT(*)` y `COUNT(col)` devuelven **0**, pero `SUM`, `AVG`, `MIN` y `MAX` devuelven **`NULL`**. En una API eso suele romper el mapeo a `decimal` no anulable; usa `COALESCE(SUM(...), 0)`.

**Tipo del resultado**:

- `AVG` y `SUM` devuelven el tipo de la columna "ampliado": sobre `int`, `SUM` devuelve `int` (puede desbordar, error 8115) y `AVG` devuelve **`int` truncado**: el promedio de 1, 2 y 2 es 1, no 1.67. Convierte antes: `AVG(CAST(Cantidad AS decimal(10,2)))` o `AVG(Cantidad * 1.0)`.
- `SUM` de `decimal(10,2)` devuelve `decimal(38,2)`, sin perder centavos.
- `COUNT` devuelve `int` (máximo 2,147,483,647). Si puedes contar más filas que eso (tablas de logs o telemetría), usa **`COUNT_BIG`**, que devuelve `bigint`. También es obligatorio `COUNT_BIG(*)` en vistas indexadas con `GROUP BY`.

**`MIN` y `MAX`** funcionan con números, fechas y textos (orden según collation).

### Ejemplo
```sql
-- Resumen de ventas no canceladas en una sola fila
SELECT COUNT(*)                                       AS Renglones,
       COUNT(DISTINCT d.PedidoId)                     AS Pedidos,
       SUM(d.Cantidad * d.PrecioUnitario)             AS Importe,
       AVG(d.Cantidad * d.PrecioUnitario)             AS ImportePromedioRenglon,
       AVG(d.Cantidad)                                AS CantidadPromedio_Truncada,
       AVG(CAST(d.Cantidad AS decimal(10,2)))         AS CantidadPromedio_Real,
       MIN(p.Fecha)                                   AS PrimerPedido,
       MAX(p.Fecha)                                   AS UltimoPedido
FROM   ventas.PedidoDetalle d
JOIN   ventas.Pedidos p ON p.PedidoId = d.PedidoId
WHERE  p.Estado <> 'Cancelado';

-- Agregado sobre cero filas: SUM da NULL, COUNT da 0
SELECT COUNT(*) AS Filas, SUM(Precio) AS Suma, COALESCE(SUM(Precio), 0) AS SumaSegura
FROM   ventas.Productos
WHERE  Categoria = N'No existe';      -- 0, NULL, 0.00

-- Conteo de una tabla gigante
SELECT COUNT_BIG(*) AS Eventos FROM dbo.Telemetria;
```

Con Dapper, un resumen se mapea a un `record`; declara anulables los agregados que pueden venir `NULL` si no usas `COALESCE`:

```csharp
public sealed record ResumenVentas(int Renglones, int Pedidos, decimal Importe, DateTime? UltimoPedido);

var resumen = await conexion.QuerySingleAsync<ResumenVentas>(sqlResumen, new { Desde = desde });
```

### Cuándo usarlo y cuándo no
- **Agregados con `GROUP BY`** cuando quieres una fila por grupo y no necesitas el detalle.
- **Funciones de ventana** (`SUM(...) OVER (...)`) cuando quieres el total *junto* a cada fila del detalle, sin colapsarlas (ver [[Funciones de ventana]] y [[Totales acumulados con OVER]]).
- **`STRING_AGG`** para concatenar textos de un grupo (ver [[STRING_AGG y STRING_SPLIT]]).
- Para "¿existe al menos una fila?" no uses `COUNT(*) > 0`; `EXISTS` se detiene en la primera.
- Evita `AVG` de valores ya promediados (promedio de promedios): pondera o calcula desde el detalle.

### Errores comunes
1. **División entera** en `AVG` sobre `int`: el resultado se trunca sin aviso.
2. **No proteger el `NULL`** de `SUM`/`AVG`/`MAX` sobre cero filas, que luego truena en C# al mapear a un tipo no anulable.
3. **Creer que `COUNT(col)` cuenta todas las filas**: omite los `NULL`.
4. **Sumar después de un JOIN que multiplica filas** (padre con dos hijos): importes inflados. Agrega cada hijo por separado antes de unirlos.
5. **Desbordamiento** de `SUM` sobre `int` o de `COUNT` con más de 2^31-1 filas: usa `bigint`/`COUNT_BIG`.

### Para profundizar
- [Funciones de agregado (Transact-SQL)](https://learn.microsoft.com/sql/t-sql/functions/aggregate-functions-transact-sql)
- [AVG (Transact-SQL)](https://learn.microsoft.com/sql/t-sql/functions/avg-transact-sql)
- [COUNT_BIG (Transact-SQL)](https://learn.microsoft.com/sql/t-sql/functions/count-big-transact-sql)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Con la base Tienda ([[Lab 1 - Base de práctica Tienda en SQL Server]]): sobre `ventas.PedidoDetalle`, número de renglones, importe total, importe promedio por renglón y precios unitarios mínimo y máximo.
> ```sql
> SELECT COUNT(*)                          AS Renglones,
>        SUM(Cantidad * PrecioUnitario)    AS Total,
>        AVG(Cantidad * PrecioUnitario)    AS PromedioRenglon,
>        MIN(PrecioUnitario)               AS PrecioMin,
>        MAX(PrecioUnitario)               AS PrecioMax
> FROM   ventas.PedidoDetalle;
> ```
> 12, 58950.00, 4912.50, 150.00, 18500.00 (incluye el pedido cancelado).

> [!question]- Calcula el promedio de `Cantidad` en `PedidoDetalle`: primero tal cual y luego correcto.
> ```sql
> SELECT AVG(Cantidad)                      AS Truncado,  -- 1
>        AVG(CAST(Cantidad AS decimal(10,2))) AS Correcto   -- 1.500000
> FROM   ventas.PedidoDetalle;
> ```
> Suman 18 unidades en 12 renglones.

> [!question]- Precio promedio del catálogo y diferencia entre el producto más caro y el más barato.
> ```sql
> SELECT AVG(Precio) AS Promedio, MAX(Precio) - MIN(Precio) AS Rango
> FROM   ventas.Productos;
> ```
> 4400.00 y 18350.00.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué devuelve `SUM(Monto)` sobre cero filas? ¿Y `COUNT(*)`?
> `SUM` devuelve `NULL`; `COUNT(*)` devuelve `0`. Por eso se suele escribir `ISNULL(SUM(Monto), 0)` o `COALESCE`.

> [!question]- ¿Por qué `AVG(Cantidad)` sobre una columna `int` puede dar un resultado «raro»?
> Con `int`, `AVG` devuelve `int` y trunca: el promedio de 1 y 2 da 1. Convierte antes: `AVG(CAST(Cantidad AS decimal(10,2)))` o `AVG(Cantidad * 1.0)`.

> [!question]- ¿Qué agregados ignoran `NULL` y cuál no?
> `SUM`, `AVG`, `MIN`, `MAX` y `COUNT(columna)` ignoran `NULL`. `COUNT(*)` cuenta filas. Ojo: `AVG` de (10, `NULL`, 20) es 15, no 10.

> [!question]- ¿Cuándo necesitas `COUNT_BIG`?
> Cuando el conteo puede superar 2,147,483,647 (límite de `int`), o en vistas indexadas con `GROUP BY`, que lo exigen.

## Práctica

- [[Lab 2 - Consultas de agregación y JOINs en Tienda]]

