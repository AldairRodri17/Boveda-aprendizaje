---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
---
# DAX - FILTER, ALL, ALLEXCEPT y REMOVEFILTERS

Parte de: [[17-Power-BI/04 DAX/_Índice DAX|DAX]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Son las funciones con las que decides **qué filas ve un cálculo**:
- `FILTER(tabla, condición)`: iterador que devuelve las filas de una tabla que cumplen una condición (puede usar medidas).
- `ALL(tabla | columnas)`: devuelve todas las filas o valores **ignorando filtros**; como argumento de CALCULATE, quita filtros.
- `ALLEXCEPT(tabla, columnas)`: quita los filtros de una tabla **excepto** los de las columnas indicadas.
- `REMOVEFILTERS(tabla | columnas)`: solo quita filtros; es un alias más legible de ALL **usado como modificador** de CALCULATE (no devuelve tabla).
- `ALLSELECTED`: quita los filtros internos del visual pero respeta lo seleccionado fuera de él (segmentaciones).

### Cómo funciona
- `FILTER` crea un contexto de fila, evalúa la condición por fila y regresa una tabla. Como filtro de CALCULATE, esa tabla **reemplaza** los filtros de las columnas que contiene. Si le das `Ventas` completa, la tabla filtro incluye todas sus columnas (pesado y con efectos colaterales).
- `ALL(Productos)` como modificador borra los filtros de todas las columnas de Productos; `ALL(Productos[Categoria])` solo los de esa columna.
- `ALLEXCEPT(Productos, Productos[Categoria])` equivale a quitar filtros de todas las columnas de Productos salvo Categoria: útil para "total de la categoría" en la fila de un producto.
- Diferencia ALL vs REMOVEFILTERS: ALL puede usarse como **tabla** (`COUNTROWS(ALL(Productos))` = 3) y como modificador; REMOVEFILTERS solo como modificador. Dentro de CALCULATE hacen lo mismo; REMOVEFILTERS comunica mejor la intención.

### Ejemplo
> [!example] Modelo de ejemplo de esta sección
> - `Productos`: P1 Café (Bebidas, Negro), P2 Molino (Equipo, Rojo), P3 Galletas (Alimentos, Café).
> - `Ventas` (Cantidad × PrecioUnitario, CostoUnitario): 1) 2024-01-15 P1 2×100 (60); 2) 2024-02-10 P2 1×250 (150); 3) 2024-02-20 P1 3×100 (60); 4) 2025-01-05 P3 5×40 (25); 5) 2025-02-14 P2 2×250 (150).
> - `Calendario` (Fecha, Año, NumMes, Mes, AñoMes, Trimestre) relacionado 1:* con `Ventas[Fecha]`; `Productos` 1:* con `Ventas[IdProducto]`.
> - Medida base: `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` → total **1 450**; Bebidas 500, Equipo 750, Alimentos 200; 2024 = 750, 2025 = 700.

```dax
-- % del producto dentro de su categoría
% en la categoria =
DIVIDE(
    [Ventas $],
    CALCULATE([Ventas $], ALLEXCEPT(Productos, Productos[Categoria]))
)
-- Cada categoría tiene un solo producto → 100 % en cada fila de producto

-- Suma de líneas con importe > 300 (condición por línea → FILTER sobre Ventas)
Ventas grandes =
SUMX(
    FILTER(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario] > 300),
    Ventas[Cantidad] * Ventas[PrecioUnitario]
)
-- Importes: 200, 250, 300, 200, 500 → solo 500

-- Ignorar todos los filtros del modelo
Ventas todas = CALCULATE([Ventas $], REMOVEFILTERS())   -- 1 450 siempre

-- Productos con ventas mayores a 400 (condición con medida → FILTER sobre dimensión)
Ventas productos top =
CALCULATE([Ventas $], FILTER(VALUES(Productos[IdProducto]), [Ventas $] > 400))
-- Café 500 + Molino 750 = 1 250
```

Fíjate en el último: la condición usa una medida, así que hace falta FILTER, pero se itera la **columna** `Productos[IdProducto]` (3 valores), no la tabla Ventas.

### Cuándo usarlo y cuándo no
- **Filtro simple sobre una columna** → filtro booleano en CALCULATE (`Productos[Color] = "Rojo"`), no FILTER.
- **Condición con medida o con columnas de varias tablas** → `FILTER` sobre la **tabla o columna más pequeña** posible.
- **Porcentaje del total** → `REMOVEFILTERS(Dim)`; **del total visible** → `ALLSELECTED(Dim)`.
- **Total del grupo padre** → `ALLEXCEPT` o `REMOVEFILTERS` de la columna hija.

### Errores comunes
- **`FILTER(Ventas, Productos[Categoria] = "Bebidas")`**: no funciona sin RELATED y, aunque lo agregues, itera millones de filas. Basta `CALCULATE([Ventas $], Productos[Categoria] = "Bebidas")`.
- **`FILTER(ALL(Ventas), ...)`**: además de lento, quita *todos* los filtros de Ventas, incluidos los que llegan de otras dimensiones, y cambia el resultado.
- **ALLEXCEPT sobre la tabla de hechos** (`ALLEXCEPT(Ventas, Productos[Categoria])`): Ventas "incluye" a sus dimensiones relacionadas, así que quita también los filtros de Calendario y de cualquier otra dimensión, no solo los de Productos. Aplícalo a la dimensión que quieres liberar o usa `REMOVEFILTERS(Productos[Producto])`, que es más predecible.
- **Usar ALL donde querías ALLSELECTED**: el porcentaje ignora la segmentación y no suma 100 % en el visual.

### Para profundizar
- [Función FILTER](https://learn.microsoft.com/en-us/dax/filter-function-dax)
- [Función ALLEXCEPT](https://learn.microsoft.com/en-us/dax/allexcept-function-dax)
- [Función REMOVEFILTERS](https://learn.microsoft.com/en-us/dax/removefilters-function-dax)
- [Evitar FILTER como argumento de filtro](https://learn.microsoft.com/en-us/dax/best-practices/dax-avoid-avoid-filter-as-filter-argument)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Datos de ejemplo (para los ejercicios con resultado numérico): Productos P1 Café (Bebidas), P2 Molino (Equipo), P3 Galletas (Alimentos). Ventas: 1) 2024-01-15 P1 2×100; 2) 2024-02-10 P2 1×250; 3) 2024-02-20 P1 3×100; 4) 2025-01-05 P3 5×40; 5) 2025-02-14 P2 2×250. Medida base: `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` → total 1 450. Medida `% en la categoría` (producto entre su categoría).
> ```dax
> % en la categoría =
> DIVIDE([Ventas $], CALCULATE([Ventas $], ALLEXCEPT(Productos, Productos[Categoria])))
> ```

> [!question]- Medida `Ventas grandes`: suma de ventas cuyo importe por línea sea mayor a 300. ¿Cuánto da?
> ```dax
> Ventas grandes =
> CALCULATE([Ventas $], FILTER(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario] > 300))
> ```
> Solo la venta 5 (500) supera 300 → 500.

> [!question]- Medida `Ventas todas` que ignore cualquier filtro del modelo.
> ```dax
> Ventas todas = CALCULATE([Ventas $], REMOVEFILTERS())
> ```
> = 1 450.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué devuelve FILTER?
> Una tabla con las filas de la tabla de entrada que cumplen la condición. Es un iterador (contexto de fila).

> [!question]- Diferencia entre ALL y REMOVEFILTERS.
> Como modificador de CALCULATE hacen lo mismo (quitar filtros). ALL además puede usarse como función de tabla (devuelve todas las filas/valores); REMOVEFILTERS solo como modificador, por eso es más legible.

> [!question]- ¿Qué hace `ALLEXCEPT(Productos, Productos[Categoria])`?
> Quita todos los filtros de la tabla Productos excepto el de Categoría. Útil para "% dentro de la categoría".

> [!question]- Error común: `FILTER(Ventas, ...)` sobre una tabla de hechos enorme cuando bastaba filtrar una columna.
> Iterar toda la tabla es lento. Prefiere filtros sobre columnas: `KEEPFILTERS(Ventas[Cantidad] > 3)` o `FILTER(VALUES(col), ...)`.

## Práctica

- [[Lab 6 - Medidas con CALCULATE y comparativos de tiempo en Power BI]]

