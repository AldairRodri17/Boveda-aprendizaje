---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
---
# Power Query - columnas condicionales y personalizadas

Parte de: [[17-Power-BI/02 Power Query y el lenguaje M/_Índice Power Query y el lenguaje M|Power Query y el lenguaje M]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Power Query ofrece dos formas de **agregar columnas calculadas por fila** antes de cargar al modelo:

- **Columna condicional**: un asistente sin código para reglas tipo *si… entonces… si no…* sobre columnas existentes.
- **Columna personalizada**: escribes una expresión M libre (aritmética, texto, fechas, `if`, llamadas a funciones).

Las dos generan el mismo paso `Table.AddColumn`; la condicional es solo una interfaz que escribe el `if` por ti.

### Cómo funciona
```m
Table.AddColumn(tabla, "NombreNueva", each expresión, tipo)
```

- **`each`** es azúcar sintáctica de una función de un parámetro: `each [Cantidad] * [Precio]` equivale a `(_) => _[Cantidad] * _[Precio]`. Power Query llama esa función **una vez por fila**, pasando la fila como registro `_`, y `[Cantidad]` es el campo `Cantidad` de esa fila.
- El cuarto argumento **tipo** evita que la columna quede como `any`. La interfaz de columna personalizada no siempre lo pone; agrégalo tú.
- En M el `if` siempre lleva `else`, y los operadores son `and`, `or`, `not`; la igualdad es `=` y la desigualdad `<>`.
- `null` se propaga: `null * 5` es `null`. Usa `??` (coalescencia) para valores por omisión: `[Descuento] ?? 0`.

### Ejemplo
Tabla `Ventas` con `Fecha`, `Cantidad`, `PrecioUnitario`.

```m
let
    // Importe = Cantidad × PrecioUnitario, como número decimal
    ConImporte = Table.AddColumn(Ventas, "Importe",
        each [Cantidad] * [PrecioUnitario], type number),

    // Rango: Alto ≥ 1000, Medio ≥ 300, Bajo en otro caso (lo que genera "Columna condicional")
    ConRango = Table.AddColumn(ConImporte, "Rango", each
        if [Importe] >= 1000 then "Alto"
        else if [Importe] >= 300 then "Medio"
        else "Bajo", type text),

    // Trimestre con formato T1-2025
    ConTrimestre = Table.AddColumn(ConRango, "Trimestre", each
        "T" & Text.From(Date.QuarterOfYear([Fecha])) & "-" & Text.From(Date.Year([Fecha])),
        type text)
in
    ConTrimestre
```

Otra muy útil: **Columna a partir de ejemplos**. Escribes el resultado esperado en dos o tres filas (por ejemplo, `"GDL"` a partir de `"Guadalajara, Jal."`) y Power Query infiere la fórmula M; revisa siempre la que generó.

### Cuándo usarlo y cuándo no
**¿Power Query o columna calculada DAX?**

- **Power Query (o SQL)** cuando el valor depende solo de la **misma fila** (o de un catálogo que puedes combinar): `Importe`, `Rango`, `Trimestre`, banderas, limpieza. Se calcula una vez al actualizar, se comprime igual que una columna importada y mantiene el modelo limpio. Es la opción por defecto.
- **Columna calculada DAX** cuando necesitas información **del modelo**: valores de otra tabla vía relación (`RELATED`), agregados sobre otra tabla (`COUNTROWS(RELATEDTABLE(...))`), clasificaciones que dependen de totales del modelo, o cuando el origen es DirectQuery y no puedes transformar.
- **Medida DAX** (no columna) cuando el resultado depende del **filtro** del usuario: totales, porcentajes, promedios por periodo.

Una pista: si la columna calculada DAX solo usa columnas de su propia tabla, casi siempre debió ser una columna de Power Query.

### Errores comunes
- **No declarar el tipo** y obtener columnas `any` que se cargan como texto.
- **Olvidar el `else`** en un `if` de M (error de sintaxis); en DAX es opcional, en M no.
- **Comparar texto con distinto caso** (`"alto" = "Alto"` es falso en M); normaliza con `Text.Upper`.
- **Nulos que rompen la aritmética**: `[Cantidad] * [Precio]` da `null` si cualquiera es nulo. Decide con `??` qué significa un nulo.
- **Columnas personalizadas que llaman funciones costosas** (por ejemplo, otro origen) por cada fila: ver [[Power Query - funciones personalizadas]].

### Para profundizar
- [Agregar una columna condicional](https://learn.microsoft.com/es-mx/power-query/add-conditional-column)
- [Agregar una columna personalizada](https://learn.microsoft.com/es-mx/power-query/add-custom-column)
- [Columnas a partir de ejemplos](https://learn.microsoft.com/es-mx/power-query/column-from-example)

Relacionado: [[DAX - columnas calculadas, medidas y tablas calculadas]], [[Lenguaje M - funciones y recursividad]].

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Agrega `Importe` = Cantidad × PrecioUnitario como número decimal.
> ```m
> T1 = Table.AddColumn(Ventas, "Importe", each [Cantidad] * [PrecioUnitario], type number)
> ```

> [!question]- Agrega `Rango` : "Alto" si Importe ≥ 1000, "Medio" si ≥ 300, si no "Bajo".
> ```m
> T2 = Table.AddColumn(T1, "Rango", each
>     if [Importe] >= 1000 then "Alto"
>     else if [Importe] >= 300 then "Medio"
>     else "Bajo", type text)
> ```

> [!question]- Agrega `Trimestre` con el formato `T1-2025` a partir de `Fecha`.
> ```m
> T3 = Table.AddColumn(T2, "Trimestre", each
>     "T" & Text.From(Date.QuarterOfYear([Fecha])) & "-" & Text.From(Date.Year([Fecha])), type text)
> ```

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre columna condicional y columna personalizada?
> La condicional se arma con un asistente (si/entonces/si no); la personalizada permite escribir cualquier expresión M. Ambas generan `Table.AddColumn`.

> [!question]- ¿Qué significa `each [Cantidad] * [Precio]`?
> `each` es atajo de una función de un parámetro `(_) => _[Cantidad] * _[Precio]`; se evalúa por cada fila.

> [!question]- ¿Cuándo crear una columna en Power Query y cuándo como columna calculada DAX?
> Preferir Power Query (o el origen): se comprime mejor y se calcula al actualizar. DAX calculada solo si necesita datos del modelo ya relacionado (p. ej. con RELATED) o lógica que M no tiene fácil.

## Práctica

- [[Lab 2 - Limpieza y combinación de datos en Power Query]]

