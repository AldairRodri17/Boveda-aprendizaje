---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
---
# DAX - conteos (COUNTROWS y DISTINCTCOUNT)

Parte de: [[17-Power-BI/04 DAX/_Índice DAX|DAX]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
DAX tiene varias funciones de conteo y cada una responde una pregunta distinta:

| Función | Cuenta | BLANK |
|---|---|---|
| `COUNT(col)` | valores no vacíos de una columna (números, fechas, texto) | no lo cuenta |
| `COUNTA(col)` | valores no vacíos (incluye booleanos) | no lo cuenta |
| `COUNTBLANK(col)` | celdas vacías | solo eso |
| `COUNTROWS(tabla)` | filas de una tabla (o tabla virtual) | no aplica |
| `DISTINCTCOUNT(col)` | valores distintos | **sí** lo cuenta como un valor |
| `DISTINCTCOUNTNOBLANK(col)` | valores distintos | no lo cuenta |

### Cómo funciona
- Todas respetan el contexto de filtro: "número de ventas en 2024" es `COUNTROWS(Ventas)` evaluado con el filtro de año.
- `COUNTROWS(Ventas)` cuenta líneas directamente; `COUNT(Ventas[IdVenta])` revisa los valores de una columna y además ignora vacíos. Para "número de filas", `COUNTROWS` es más claro y no depende de qué columna elijas.
- `DISTINCTCOUNT` sobre la tabla de hechos (`Ventas[IdProducto]`) responde "¿cuántos productos distintos se vendieron?"; sobre la dimensión (`Productos[IdProducto]`) respondería "¿cuántos productos existen?", que no cambia con el año porque el filtro de Calendario no llega a Productos.
- Los conteos distintos **no son aditivos**: el total no es la suma de las filas.

### Ejemplo
> [!example] Modelo de ejemplo de esta sección
> - `Productos`: P1 Café (Bebidas, Negro), P2 Molino (Equipo, Rojo), P3 Galletas (Alimentos, Café).
> - `Ventas` (Cantidad × PrecioUnitario, CostoUnitario): 1) 2024-01-15 P1 2×100 (60); 2) 2024-02-10 P2 1×250 (150); 3) 2024-02-20 P1 3×100 (60); 4) 2025-01-05 P3 5×40 (25); 5) 2025-02-14 P2 2×250 (150).
> - `Calendario` (Fecha, Año, NumMes, Mes, AñoMes, Trimestre) relacionado 1:* con `Ventas[Fecha]`; `Productos` 1:* con `Ventas[IdProducto]`.
> - Medida base: `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` → total **1 450**; Bebidas 500, Equipo 750, Alimentos 200; 2024 = 750, 2025 = 700.

```dax
Num ventas = COUNTROWS(Ventas)
-- Total 5; 2024: líneas 1, 2 y 3 → 3; 2025: 2

Productos vendidos = DISTINCTCOUNT(Ventas[IdProducto])
-- Total: P1, P2, P3 → 3; 2024: P1, P2 → 2; 2025: P3, P2 → 2
-- Fíjate: 2 + 2 ≠ 3 (Molino se vendió ambos años)

Productos sin venta =
COUNTROWS(
    FILTER(VALUES(Productos[IdProducto]), ISBLANK([Ventas $]))
)
-- 2024: Galletas → 1; 2025: Café → 1; total: 0

-- Alternativa con conjuntos
Productos sin venta 2 =
COUNTROWS(EXCEPT(VALUES(Productos[IdProducto]), VALUES(Ventas[IdProducto])))
```

Para un cálculo de "clientes activos" usarías `DISTINCTCOUNT(Ventas[IdCliente])`; para "clientes en catálogo", `COUNTROWS(Clientes)`.

### Cuándo usarlo y cuándo no
- **COUNTROWS** para contar filas (ventas, pedidos, tickets) y para contar tablas virtuales (`COUNTROWS(FILTER(...))`).
- **DISTINCTCOUNT** para entidades distintas en la tabla de hechos (clientes, productos, días con venta). En columnas de muy alta cardinalidad es más costoso; si solo necesitas aproximación en DirectQuery existe `APPROXIMATEDISTINCTCOUNT` (verifica que tu origen lo soporte).
- **COUNT/COUNTA** cuando la pregunta es "¿cuántos valores no vacíos hay en esta columna?" (por ejemplo, ventas con fecha de envío capturada).
- **DISTINCTCOUNTNOBLANK** si la columna puede traer vacíos que no quieres contar.

### Errores comunes
- **Contar la dimensión esperando ver el efecto de un filtro de fecha**: `COUNTROWS(Productos)` da 3 en todos los años; cuenta en la tabla de hechos o filtra con la medida.
- **Sumar conteos distintos** (o esperar que el total sea la suma): los mismos elementos aparecen en varias filas.
- **DISTINCTCOUNT con blanks**: si hay ventas sin producto, el blank cuenta como un valor más; usa DISTINCTCOUNTNOBLANK o corrige la integridad.
- **Elegir COUNT sobre una columna que puede tener vacíos** para contar filas: subcuenta.

### Para profundizar
- [Función COUNTROWS](https://learn.microsoft.com/en-us/dax/countrows-function-dax)
- [Función DISTINCTCOUNT](https://learn.microsoft.com/en-us/dax/distinctcount-function-dax)
- [Funciones de conteo en DAX Guide](https://dax.guide/countrows/)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Datos de ejemplo (para los ejercicios con resultado numérico): Productos P1 Café (Bebidas), P2 Molino (Equipo), P3 Galletas (Alimentos). Ventas: 1) 2024-01-15 P1 2×100; 2) 2024-02-10 P2 1×250; 3) 2024-02-20 P1 3×100; 4) 2025-01-05 P3 5×40; 5) 2025-02-14 P2 2×250. Medida base: `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` → total 1 450. Medida `Num ventas`. ¿Cuánto da en 2024?
> ```dax
> Num ventas = COUNTROWS(Ventas)
> ```
> 2024 → 3.

> [!question]- Medida `Productos vendidos` (distintos). ¿Total y 2025?
> ```dax
> Productos vendidos = DISTINCTCOUNT(Ventas[IdProducto])
> ```
> Total 3; 2025 → 2 (P3 y P2).

> [!question]- Medida `Productos sin venta` en el contexto actual.
> ```dax
> Productos sin venta =
> COUNTROWS(FILTER(VALUES(Productos[IdProducto]), ISBLANK([Ventas $])))
> ```
> 2025 → 1 (P1).

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Diferencia entre COUNT, COUNTA, COUNTROWS y DISTINCTCOUNT?
> COUNT: valores no vacíos de una columna numérica/fecha. COUNTA: no vacíos de cualquier tipo. COUNTROWS: filas de una tabla. DISTINCTCOUNT: valores distintos de una columna.

> [!question]- ¿Qué conviene para "número de ventas": COUNT(Ventas[IdVenta]) o COUNTROWS(Ventas)?
> COUNTROWS(Ventas): expresa la intención (filas) y no depende de que una columna tenga vacíos.

> [!question]- ¿DISTINCTCOUNT cuenta el blank?
> Sí, cuenta el blank como un valor si existe. `DISTINCTCOUNTNOBLANK` no lo cuenta.

