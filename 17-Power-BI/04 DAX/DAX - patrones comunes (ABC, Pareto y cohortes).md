---
proyecto: Conocimiento
tipo: concepto
nivel: avanzado
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
---
# DAX - patrones comunes (ABC, Pareto y cohortes)

Parte de: [[17-Power-BI/04 DAX/_Índice DAX|DAX]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Tres análisis de negocio que se repiten en casi todas las empresas y que en DAX se resuelven con patrones conocidos:
- **Análisis ABC**: clasificar productos (o clientes) según su aporte **acumulado** a las ventas. A: los pocos que suman la mayor parte (por ejemplo, hasta 80 %); B: el siguiente tramo (hasta 95 %); C: el resto. Los umbrales son decisión del negocio.
- **Pareto**: la visualización del ABC: barras ordenadas de mayor a menor y una línea con el **% acumulado**; muestra cuántos elementos explican la mayoría del resultado (el famoso 80/20 es una referencia, no una ley).
- **Cohortes**: agrupar clientes por el periodo de su **primera compra** y seguir su comportamiento (retención, ventas) en los periodos siguientes.

### Cómo funciona
**ABC/Pareto**:
1. Calcula las ventas del elemento actual.
2. Suma las ventas de todos los elementos con ventas **mayores o iguales** (acumulado por ranking, ver [[DAX - totales acumulados]]).
3. Divide entre el total del conjunto visible → % acumulado.
4. Clasifica con `SWITCH(TRUE(), ...)`.

Si la clase debe usarse como **eje o segmentación**, tiene que ser una columna (calculada o hecha en Power Query) o una tabla de segmentos con TREATAS; una medida no puede ir en un eje. La versión con medida es *dinámica* (cambia con los filtros); la de columna es *estática* (calculada al actualizar).

**Cohortes**:
1. Primera compra por cliente: `CALCULATE(MIN(Ventas[Fecha]), REMOVEFILTERS(Calendario))`.
2. Materialízala como columna en la dimensión Clientes (o en Power Query/SQL) y deriva el año o mes de cohorte.
3. Mide clientes activos por periodo filtrando por cohorte: retención = activos del periodo / tamaño de la cohorte.

### Ejemplo
> [!example] Modelo de ejemplo de esta sección
> - `Productos`: P1 Café (Bebidas, Negro), P2 Molino (Equipo, Rojo), P3 Galletas (Alimentos, Café).
> - `Ventas` (Cantidad × PrecioUnitario, CostoUnitario): 1) 2024-01-15 P1 2×100 (60); 2) 2024-02-10 P2 1×250 (150); 3) 2024-02-20 P1 3×100 (60); 4) 2025-01-05 P3 5×40 (25); 5) 2025-02-14 P2 2×250 (150).
> - `Calendario` (Fecha, Año, NumMes, Mes, AñoMes, Trimestre) relacionado 1:* con `Ventas[Fecha]`; `Productos` 1:* con `Ventas[IdProducto]`.
> - Medida base: `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` → total **1 450**; Bebidas 500, Equipo 750, Alimentos 200; 2024 = 750, 2025 = 700.

```dax
% acumulado =
VAR VActual = [Ventas $]
VAR Total = CALCULATE([Ventas $], ALLSELECTED(Productos[Categoria]))
VAR Acum =
    SUMX(
        FILTER(ALLSELECTED(Productos[Categoria]), CALCULATE([Ventas $]) >= VActual),
        CALCULATE([Ventas $])
    )
RETURN DIVIDE(Acum, Total)

Clase ABC =
VAR P = [% acumulado]
RETURN SWITCH(TRUE(), P <= 0.80, "A", P <= 0.95, "B", "C")
```

| Categoría | Ventas | Acumulado | % acumulado | Clase |
|---|---|---|---|---|
| Equipo | 750 | 750 | 51.72 % | A |
| Bebidas | 500 | 1 250 | 86.21 % | B |
| Alimentos | 200 | 1 450 | 100 % | C |

Para el gráfico de Pareto: gráfico combinado de columnas y líneas, eje = Categoría ordenada por Ventas $ descendente, columnas = Ventas $, línea = % acumulado (eje secundario de 0 a 100 %).

**Cohortes** (supón una columna `Ventas[IdCliente]` con C1 en las líneas 1 y 4, C2 en las líneas 2 y 3, y C3 en la línea 5):

```dax
-- Columna calculada en Clientes
Primera compra = CALCULATE(MIN(Ventas[Fecha]), REMOVEFILTERS(Calendario))
Año cohorte = YEAR(Clientes[Primera compra])
-- C1 2024-01-15 → 2024; C2 2024-02-10 → 2024; C3 2025-02-14 → 2025

Clientes activos = DISTINCTCOUNT(Ventas[IdCliente])

Retencion % =
VAR TamCohorte = CALCULATE(COUNTROWS(Clientes), REMOVEFILTERS(Calendario))
RETURN DIVIDE([Clientes activos], TamCohorte)
-- Cohorte 2024 en el año 2025: activos = C1 → 1 / 2 = 50 %
```

### Cuándo usarlo y cuándo no
- **ABC** para inventarios (dónde concentrar el control), surtido y priorización de clientes. Hazlo por producto, no por categoría, cuando haya cientos de productos.
- **Pareto** para comunicar concentración; en conjuntos con pocos elementos aporta poco.
- **Cohortes** para medir retención en negocios recurrentes (suscripciones, clientes frecuentes). Necesitas un identificador de cliente confiable.
- Si el cálculo es pesado y no necesita ser dinámico, materializa la clase o la cohorte en Power Query o en SQL.

### Errores comunes
- **Usar ALL en vez de ALLSELECTED** en el total: los porcentajes no suman 100 % cuando el usuario filtra.
- **Empates** en el acumulado: dos elementos con el mismo valor reciben el mismo % acumulado.
- **Querer poner la clase ABC (medida) en un eje**: necesitas columna o tabla de segmentos.
- **Primera compra sin quitar el filtro de fechas**: devuelve la primera compra *dentro del periodo filtrado*, no la histórica.

### Para profundizar
- [ABC classification (DAX Patterns)](https://www.daxpatterns.com/abc-classification/)
- [New and returning customers (DAX Patterns)](https://www.daxpatterns.com/new-and-returning-customers/)
- [DAX Patterns (índice)](https://www.daxpatterns.com/)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Datos de ejemplo (para los ejercicios con resultado numérico): Productos P1 Café (Bebidas), P2 Molino (Equipo), P3 Galletas (Alimentos). Ventas: 1) 2024-01-15 P1 2×100; 2) 2024-02-10 P2 1×250; 3) 2024-02-20 P1 3×100; 4) 2025-01-05 P3 5×40; 5) 2025-02-14 P2 2×250. Medida base: `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` → total 1 450. Medida `% acumulado` de categorías ordenadas por ventas (Pareto). Valores.
> ```dax
> % acumulado =
> VAR VActual = [Ventas $]
> VAR Total = CALCULATE([Ventas $], ALLSELECTED(Productos[Categoria]))
> VAR Acum = SUMX(
>     FILTER(ALLSELECTED(Productos[Categoria]),
>         CALCULATE([Ventas $]) >= VActual),
>     CALCULATE([Ventas $]))
> RETURN DIVIDE(Acum, Total)
> ```
> Equipo 51.72 %; Bebidas 86.21 %; Alimentos 100 %.

> [!question]- Columna o medida de clase ABC usando el % acumulado (A ≤ 80 %, B ≤ 95 %, C resto). ¿Qué clase toma cada categoría?
> ```dax
> Clase ABC =
> VAR P = [% acumulado]
> RETURN SWITCH(TRUE(), P <= 0.80, "A", P <= 0.95, "B", "C")
> ```
> Equipo A; Bebidas B; Alimentos C.

> [!question]- Medida `Primera compra` del cliente (para cohortes), con Ventas[IdCliente].
> ```dax
> Primera compra = CALCULATE(MIN(Ventas[Fecha]), REMOVEFILTERS(Calendario))
> ```
> Evaluada por cliente; luego se materializa como columna en una dimensión Clientes para agrupar por cohorte.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es un análisis ABC?
> Clasificar productos por su aporte acumulado: A ≈ primeros que suman ~70–80 % de las ventas, B el siguiente tramo, C el resto. Los umbrales son una decisión de negocio.

> [!question]- ¿Qué muestra un gráfico de Pareto?
> Barras ordenadas de mayor a menor y una línea con el % acumulado; ayuda a ver qué pocos elementos generan la mayoría.

> [!question]- ¿Qué es un análisis de cohortes?
> Agrupar clientes por la fecha de su primera compra (cohorte) y seguir su comportamiento (retención, ventas) en los periodos siguientes.

