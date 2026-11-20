---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
---
# DAX - IF, SWITCH y DIVIDE

Parte de: [[17-Power-BI/04 DAX/_Índice DAX|DAX]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Son las herramientas de **lógica condicional** y de **división segura**:
- `IF(condición, valor_si_verdadero [, valor_si_falso])`: si omites el tercer argumento, devuelve BLANK.
- `SWITCH(expresión, valor1, resultado1, ..., [otro])`: compara una expresión contra varios valores; con `SWITCH(TRUE(), cond1, r1, cond2, r2, ..., otro)` evalúa condiciones en orden, como un `CASE WHEN` de SQL.
- `DIVIDE(numerador, denominador [, alterno])`: divide y, si el denominador es 0 o BLANK, devuelve el alterno (BLANK por omisión) en lugar de error o infinito.

### Cómo funciona
- `IF` solo evalúa la rama que se cumple (evaluación perezosa en la mayoría de los casos), por eso conviene poner las condiciones baratas primero.
- `SWITCH(TRUE(), ...)` recorre las condiciones de arriba abajo y se queda con **la primera** verdadera; el orden de los umbrales importa (de mayor a menor en un semáforo).
- `DIVIDE` existe porque el operador `/` con denominador 0 da infinito o NaN, y con BLANK puede dar BLANK o infinito según el caso; DIVIDE lo resuelve en una función legible y optimizada.
- **BLANK vs 0**: los visuales esconden filas cuyas medidas son BLANK. Si una medida devuelve 0 en vez de BLANK (por ejemplo `IF(ISBLANK(x), 0, x)` o `+ 0`), la matriz mostrará **todas las combinaciones** de dimensiones, aunque no tengan datos: miles de filas en cero, visuales lentos y difíciles de leer.

### Ejemplo
> [!example] Modelo de ejemplo de esta sección
> - `Productos`: P1 Café (Bebidas, Negro), P2 Molino (Equipo, Rojo), P3 Galletas (Alimentos, Café).
> - `Ventas` (Cantidad × PrecioUnitario, CostoUnitario): 1) 2024-01-15 P1 2×100 (60); 2) 2024-02-10 P2 1×250 (150); 3) 2024-02-20 P1 3×100 (60); 4) 2025-01-05 P3 5×40 (25); 5) 2025-02-14 P2 2×250 (150).
> - `Calendario` (Fecha, Año, NumMes, Mes, AñoMes, Trimestre) relacionado 1:* con `Ventas[Fecha]`; `Productos` 1:* con `Ventas[IdProducto]`.
> - Medida base: `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` → total **1 450**; Bebidas 500, Equipo 750, Alimentos 200; 2024 = 750, 2025 = 700.

Supón `Costo $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[CostoUnitario])` y `% Margen = DIVIDE([Ventas $] - [Costo $], [Ventas $])`.

```dax
Semaforo =
VAR M = [% Margen]
RETURN
    IF(
        NOT ISBLANK([Ventas $]),
        SWITCH(TRUE(),
            M >= 0.30, "Verde",
            M >= 0.15, "Amarillo",
            "Rojo")
    )
-- Bebidas (500−300)/500 = 40 % Verde; Equipo (750−450)/750 = 40 % Verde;
-- Alimentos (200−125)/200 = 37.5 % Verde; Bebidas en 2025: blank

Precio promedio = DIVIDE([Ventas $], SUM(Ventas[Cantidad]))
-- 1 450 / 13 = 111.54; en un periodo sin unidades devuelve blank, no error

-- Columna en Calendario: NumTrimestre = QUARTER(Calendario[Fecha])
Nombre trimestre =
SWITCH(
    SELECTEDVALUE(Calendario[NumTrimestre]),
    1, "Primero",
    2, "Segundo",
    3, "Tercero",
    4, "Cuarto",
    "Varios"
)
```

`Nombre trimestre` usa el SWITCH simple (compara un valor contra constantes); si hay varios trimestres seleccionados, `SELECTEDVALUE` devuelve BLANK y cae en "Varios" ([[DAX - SELECTEDVALUE, ISINSCOPE y HASONEVALUE]]).

### Cuándo usarlo y cuándo no
- **DIVIDE** siempre que el denominador pueda ser 0 o BLANK (porcentajes, promedios, crecimientos). Si sabes que el denominador es una constante distinta de cero, `/` es aceptable y un poco más directo.
- **SWITCH(TRUE())** para tramos o reglas de negocio con más de dos ramas; más legible que IF anidados.
- **IF sin tercer argumento** para devolver BLANK cuando no aplica el cálculo (por ejemplo, ocultar el valor en totales).
- Si la clasificación es fija por fila (rango de precio de un producto) y debe usarse en un eje, hazla **columna** en Power Query o columna calculada, no medida.

### Errores comunes
- **Devolver 0 o "" en lugar de BLANK** sin necesidad: el visual llena filas vacías y pierde rendimiento.
- **Umbrales en orden incorrecto** en SWITCH(TRUE()): si pones `M >= 0.15` antes que `M >= 0.30`, nunca saldrá "Verde".
- **`IF(denominador = 0, ...)`** cuando el denominador es BLANK: funciona porque BLANK = 0 es TRUE, pero DIVIDE es más claro.
- **Comparar textos con mayúsculas distintas**: en DAX las comparaciones de texto no distinguen mayúsculas, pero el resultado que devuelves sí se muestra tal cual.

### Para profundizar
- [Función IF](https://learn.microsoft.com/en-us/dax/if-function-dax)
- [Función SWITCH](https://learn.microsoft.com/en-us/dax/switch-function-dax)
- [DIVIDE vs operador de división (guía)](https://learn.microsoft.com/en-us/dax/best-practices/dax-divide-function-operator)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Medida `Semáforo` : "Verde" si % Margen ≥ 30 %, "Amarillo" si ≥ 15 %, si no "Rojo"; blank si no hay ventas.
> ```dax
> Semáforo =
> VAR M = [% Margen]
> RETURN
> SWITCH(TRUE(),
>     ISBLANK(M), BLANK(),
>     M >= 0.30, "Verde",
>     M >= 0.15, "Amarillo",
>     "Rojo")
> ```

> [!question]- Medida `Precio promedio` = Ventas $ / unidades, segura ante cero.
> ```dax
> Precio promedio = DIVIDE([Ventas $], SUM(Ventas[Cantidad]))
> ```
> Con los datos de ejemplo: 1 450 / 13 ≈ 111.54.

> [!question]- Medida `Nombre trimestre` que devuelva "Primero", "Segundo", "Tercero" o "Cuarto" según el trimestre seleccionado (SWITCH simple). Supón la columna `Calendario[NumTrimestre] = QUARTER([Fecha])`.
> ```dax
> Nombre trimestre =
> SWITCH(SELECTEDVALUE(Calendario[NumTrimestre]),
>     1, "Primero", 2, "Segundo", 3, "Tercero", 4, "Cuarto",
>     "Varios")
> ```

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Por qué usar DIVIDE en vez de `/`?
> DIVIDE devuelve blank (o un valor alterno) si el denominador es 0 o blank, en vez de error/infinito.

> [!question]- ¿Qué es el patrón `SWITCH(TRUE(), ...)`?
> Un IF múltiple legible: evalúa condiciones en orden y devuelve el primer resultado verdadero.

> [!question]- ¿Qué pasa si una medida devuelve 0 en vez de blank en todas las filas?
> Los visuales muestran todas las combinaciones (aunque no tengan datos), lo que llena tablas y empeora rendimiento. Evita `+ 0` innecesarios.

## Práctica

- [[Lab 6 - Medidas con CALCULATE y comparativos de tiempo en Power BI]]

