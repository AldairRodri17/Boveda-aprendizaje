---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
contenido: completo
---
# DAX - RANKX y TOPN

Parte de: [[17-Power-BI/04 DAX/_Índice DAX|DAX]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
- `RANKX(tabla, expresión [, valor, orden, empates])` es un iterador que calcula la **posición** del elemento actual dentro de una tabla, ordenada por una expresión.
- `TOPN(n, tabla, expresión [, orden])` devuelve una **tabla** con las N filas de mayor (o menor) valor; se usa como filtro o como tabla virtual.
- Funciones de ventana más recientes: `RANK`, `ROWNUMBER`, `WINDOW`, `INDEX` y `OFFSET`, que permiten ordenar por varias columnas y particionar.

### Cómo funciona
**RANKX** en dos pasos:
1. Evalúa la expresión en el **contexto actual** (el producto de la fila): ese es el valor a rankear.
2. Itera la tabla del primer argumento, evalúa la expresión para cada fila (con transición de contexto si es medida) y cuenta cuántas filas superan el valor.

Por eso la tabla debe contener **a todos los competidores**. Si escribes `RANKX(VALUES(Productos[Producto]), [Ventas $])`, en la fila "Café" VALUES solo tiene a Café, y el ranking es 1 para todos. Hay que iterar `ALL(Productos[Producto])` (todos) o `ALLSELECTED(Productos[Producto])` (los visibles en el visual).

**Empates**: el quinto argumento es `Skip` (por omisión: 1, 1, 3) o `Dense` (1, 1, 2).

**TOPN** no garantiza N filas exactas: si hay empate en la posición N, devuelve todas las empatadas.

### Ejemplo
> [!example] Modelo de ejemplo de esta sección
> - `Productos`: P1 Café (Bebidas, Negro), P2 Molino (Equipo, Rojo), P3 Galletas (Alimentos, Café).
> - `Ventas` (Cantidad × PrecioUnitario, CostoUnitario): 1) 2024-01-15 P1 2×100 (60); 2) 2024-02-10 P2 1×250 (150); 3) 2024-02-20 P1 3×100 (60); 4) 2025-01-05 P3 5×40 (25); 5) 2025-02-14 P2 2×250 (150).
> - `Calendario` (Fecha, Año, NumMes, Mes, AñoMes, Trimestre) relacionado 1:* con `Ventas[Fecha]`; `Productos` 1:* con `Ventas[IdProducto]`.
> - Medida base: `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` → total **1 450**; Bebidas 500, Equipo 750, Alimentos 200; 2024 = 750, 2025 = 700.

```dax
Ranking categoria = RANKX(ALL(Productos[Categoria]), [Ventas $])
-- Equipo 750 → 1; Bebidas 500 → 2; Alimentos 200 → 3

Ventas top 2 =
CALCULATE([Ventas $], TOPN(2, ALL(Productos[Categoria]), [Ventas $]))
-- Equipo + Bebidas = 1 250

Ranking producto =
IF(
    ISINSCOPE(Productos[Producto]),
    RANKX(ALLSELECTED(Productos[Producto]), [Ventas $], , DESC, Dense)
)
-- Molino 1, Café 2, Galletas 3; blank en subtotales y total

-- Función de ventana RANK (sin iterar con medida en el primer argumento)
Ranking con RANK =
RANK(DENSE, ALLSELECTED(Productos[Producto]), ORDERBY([Ventas $], DESC))
```

Ejemplo de empate: si en 2025 Café y Galletas tuvieran ambas 200, con `Skip` quedarían 2, 2 y no habría 3 (el siguiente sería 4); con `Dense` serían 2, 2, 3.

Tabla virtual con TOPN para un texto dinámico:

```dax
Top 2 nombres =
CONCATENATEX(
    TOPN(2, ALL(Productos[Producto]), [Ventas $]),
    Productos[Producto], ", ", [Ventas $], DESC
)
-- "Molino, Café"
```

### Cuándo usarlo y cuándo no
- **RANKX** para una columna de ranking en tablas y matrices, o para filtrar "Top N" con un parámetro.
- **TOPN** para cálculos sobre los N mejores (ventas del top 10, % que representan) y para títulos dinámicos.
- **RANK/WINDOW** cuando necesitas ordenar por varias columnas o desempatar con un criterio secundario de forma explícita.
- Para un Top N simple en un solo visual, el **filtro Top N del panel de filtros** no requiere DAX.

### Errores comunes
- **Iterar VALUES en lugar de ALL/ALLSELECTED**: ranking 1 para todos.
- **Rankear en los totales**: el total también recibe un número sin sentido; protégelo con `ISINSCOPE`.
- **Usar ALL cuando hay segmentación**: el ranking incluye elementos que el usuario no ve (por ejemplo, posición 7 de una lista visible de 5). Usa ALLSELECTED.
- **Ignorar empates en TOPN**: "Top 2" puede traer 3 filas.
- **Rankear con una expresión que no es medida** sin CALCULATE: sin transición de contexto todos tienen el mismo valor.

### Para profundizar
- [Función RANKX](https://learn.microsoft.com/en-us/dax/rankx-function-dax)
- [Función TOPN](https://learn.microsoft.com/en-us/dax/topn-function-dax)
- [Función RANK (funciones de ventana)](https://learn.microsoft.com/en-us/dax/rank-function-dax)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Datos de ejemplo (para los ejercicios con resultado numérico): Productos P1 Café (Bebidas), P2 Molino (Equipo), P3 Galletas (Alimentos). Ventas: 1) 2024-01-15 P1 2×100; 2) 2024-02-10 P2 1×250; 3) 2024-02-20 P1 3×100; 4) 2025-01-05 P3 5×40; 5) 2025-02-14 P2 2×250. Medida base: `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` → total 1 450. Medida `Ranking categoría` por ventas. ¿Qué rango tiene Bebidas?
> ```dax
> Ranking categoría = RANKX(ALL(Productos[Categoria]), [Ventas $])
> ```
> Equipo 750 → 1, Bebidas 500 → 2, Alimentos 200 → 3.

> [!question]- Medida `Ventas top 2 categorías`.
> ```dax
> Ventas top 2 = CALCULATE([Ventas $], TOPN(2, ALL(Productos[Categoria]), [Ventas $]))
> ```
> = 750 + 500 = 1 250.

> [!question]- Medida que muestre el ranking solo a nivel producto (blank en totales).
> ```dax
> Ranking producto =
> IF(ISINSCOPE(Productos[Producto]), RANKX(ALLSELECTED(Productos[Producto]), [Ventas $]))
> ```

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Por qué `RANKX(Productos, [Ventas $])` puede dar 1 a todos?
> Si se escribe `RANKX(VALUES(Productos[Producto]), ...)` dentro de la fila, VALUES solo tiene el producto actual. Hay que iterar todos: `ALL(Productos[Producto])` o `ALLSELECTED`.

> [!question]- ¿Qué hace TOPN?
> Devuelve una tabla con las N filas con mayor (o menor) valor de una expresión; se usa como filtro en CALCULATE o en tablas virtuales.

> [!question]- ¿Cómo maneja RANKX los empates?
> Por defecto `Skip` (1, 1, 3); con `Dense` da (1, 1, 2).

> [!question]- ¿Qué alternativa más reciente existe para ranking?
> La función de ventana `RANK` (junto con WINDOW, INDEX, OFFSET), que permite ordenar por varias columnas.

