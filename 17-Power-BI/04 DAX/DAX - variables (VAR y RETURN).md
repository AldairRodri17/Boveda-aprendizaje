---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
---
# DAX - variables (VAR y RETURN)

Parte de: [[17-Power-BI/04 DAX/_Índice DAX|DAX]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Las variables (`VAR nombre = expresión` ... `RETURN resultado`) guardan un valor escalar **o una tabla** dentro de una fórmula. Hacen el DAX más legible, evitan calcular dos veces lo mismo y facilitan depurar. Una variable se **evalúa una sola vez, en el contexto donde se define**, y a partir de ahí es una constante.

### Cómo funciona
```dax
Medida =
VAR A = <expresión 1>          -- se evalúa en el contexto actual
VAR B = <expresión que usa A>  -- puede usar variables anteriores
RETURN
    <expresión final con A y B>
```

Reglas:
1. **Se evalúan donde se escriben**, no donde se usan. Si defines `VAR V = [Ventas $]` y luego escribes `CALCULATE(V, Productos[Categoria] = "Bebidas")`, CALCULATE no puede cambiar `V`: ya es un número.
2. **Evaluación perezosa**: si una variable no se usa en el RETURN (o en una rama de IF que no se ejecuta), el motor puede no calcularla.
3. **Ámbito**: una variable solo existe dentro de la expresión donde se declara; puedes declarar variables dentro de un iterador y entonces se evalúan por fila.
4. **Pueden contener tablas**: `VAR T = FILTER(...)` y luego `COUNTROWS(T)` o `SUMX(T, ...)`. Para referirte a columnas de una tabla variable creada con ADDCOLUMNS, usa el nombre de la columna (`[@V]`).

### Ejemplo
> [!example] Modelo de ejemplo de esta sección
> - `Productos`: P1 Café (Bebidas, Negro), P2 Molino (Equipo, Rojo), P3 Galletas (Alimentos, Café).
> - `Ventas` (Cantidad × PrecioUnitario, CostoUnitario): 1) 2024-01-15 P1 2×100 (60); 2) 2024-02-10 P2 1×250 (150); 3) 2024-02-20 P1 3×100 (60); 4) 2025-01-05 P3 5×40 (25); 5) 2025-02-14 P2 2×250 (150).
> - `Calendario` (Fecha, Año, NumMes, Mes, AñoMes, Trimestre) relacionado 1:* con `Ventas[Fecha]`; `Productos` 1:* con `Ventas[IdProducto]`.
> - Medida base: `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` → total **1 450**; Bebidas 500, Equipo 750, Alimentos 200; 2024 = 750, 2025 = 700.

```dax
Costo $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[CostoUnitario])

% Margen =
VAR V = [Ventas $]
VAR C = [Costo $]
RETURN DIVIDE(V - C, V)
-- Total: (1 450 − 875) / 1 450 = 39.66 %

Crecimiento % =
VAR Actual   = [Ventas $]
VAR Anterior = CALCULATE([Ventas $], SAMEPERIODLASTYEAR(Calendario[Fecha]))
RETURN
    IF(NOT ISBLANK(Anterior), DIVIDE(Actual - Anterior, Anterior))
-- Año 2025: (700 − 750) / 750 = −6.67 %; 2024: blank (no hay 2023)
```

La trampa clásica:

```dax
Prueba =
VAR V = [Ventas $]
RETURN CALCULATE(V, Productos[Categoria] = "Bebidas")
-- En el total devuelve 1 450, no 500: V se calculó antes del CALCULATE
```

Lo correcto es poner la **medida** (o la expresión) dentro de CALCULATE: `CALCULATE([Ventas $], Productos[Categoria] = "Bebidas")`.

Depuración: cambia temporalmente el `RETURN` para devolver una variable intermedia (`RETURN Anterior`) y revisa su valor en una matriz; o, si es una tabla, pruébala con EVALUATE en la [[Vista de consultas DAX (DAX query view)]].

```dax
-- Variable de tabla
Productos sobre 400 =
VAR T = FILTER(VALUES(Productos[IdProducto]), [Ventas $] > 400)
RETURN COUNTROWS(T)   -- Café y Molino → 2
```

### Cuándo usarlo y cuándo no
- **Casi siempre**: en cuanto una subexpresión se repite o la fórmula pasa de una línea, usa variables con nombres claros (`VentasAA`, `UltimaFecha`).
- Para "capturar" un valor del contexto externo antes de cambiarlo, como `VAR UltFecha = MAX(Calendario[Fecha])` en un acumulado ([[DAX - totales acumulados]]); sustituyen a `EARLIER`.
- No esperes que una variable "se recalcule" en otro contexto: si necesitas eso, usa una medida o repite la expresión dentro de CALCULATE.

### Errores comunes
- **`CALCULATE(variable, ...)`**: no cambia nada; error frecuentísimo.
- **Definir la variable fuera del iterador** cuando debía calcularse por fila (o al revés): revisa en qué contexto quieres el valor.
- **Nombres que chocan** con tablas o medidas; usa prefijos o nombres descriptivos.
- **Variables gigantes de tabla** materializadas sin necesidad: si solo usas un conteo, calcula el conteo directamente.

### Para profundizar
- [VAR (referencia DAX)](https://learn.microsoft.com/en-us/dax/var-dax)
- [Usar variables para mejorar fórmulas DAX](https://learn.microsoft.com/en-us/dax/best-practices/dax-variables)
- [Variables en DAX Guide](https://dax.guide/st/var/)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Datos de ejemplo (para los ejercicios con resultado numérico): Productos P1 Café (Bebidas), P2 Molino (Equipo), P3 Galletas (Alimentos). Ventas: 1) 2024-01-15 P1 2×100; 2) 2024-02-10 P2 1×250; 3) 2024-02-20 P1 3×100; 4) 2025-01-05 P3 5×40; 5) 2025-02-14 P2 2×250. Medida base: `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` → total 1 450. Reescribe con VAR: `% Margen = ([Ventas $] - [Costo $]) / [Ventas $]` evitando división entre cero.
> ```dax
> % Margen =
> VAR V = [Ventas $]
> VAR C = [Costo $]
> RETURN DIVIDE(V - C, V)
> ```

> [!question]- Medida `Crecimiento %` contra el año anterior usando variables.
> ```dax
> Crecimiento % =
> VAR Actual = [Ventas $]
> VAR Anterior = CALCULATE([Ventas $], SAMEPERIODLASTYEAR(Calendario[Fecha]))
> RETURN DIVIDE(Actual - Anterior, Anterior)
> ```
> Con los datos de ejemplo, año 2025: (700 − 750) / 750 ≈ −6.67 %.

> [!question]- ¿Qué devuelve esta medida y por qué?
```dax
Prueba =
VAR V = [Ventas $]
RETURN CALCULATE(V, Productos[Categoria] = "Bebidas")
```
> Devuelve las ventas del contexto actual (en el total, 1 450), **no** 500: la variable ya se evaluó antes del CALCULATE. Lo correcto es `CALCULATE([Ventas $], ...)`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Ventajas de VAR?
> Legibilidad, evitar repetir cálculos (se evalúa una vez) y depurar (devolver una variable en RETURN para ver su valor).

> [!question]- ¿Una variable se recalcula si después cambias el contexto con CALCULATE?
> No. Las variables se evalúan donde se definen; son constantes. `CALCULATE(v, ...)` no cambia el valor de `v`. Error frecuente.

> [!question]- ¿Se pueden guardar tablas en una variable?
> Sí: `VAR T = FILTER(...)` y luego `COUNTROWS(T)`.

## Práctica

- [[Lab 6 - Medidas con CALCULATE y comparativos de tiempo en Power BI]]

