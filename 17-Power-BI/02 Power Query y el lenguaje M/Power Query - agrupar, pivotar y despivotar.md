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
# Power Query - agrupar, pivotar y despivotar

Parte de: [[17-Power-BI/02 Power Query y el lenguaje M/_Índice Power Query y el lenguaje M|Power Query y el lenguaje M]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Tres transformaciones que **cambian la forma** de una tabla:

- **Agrupar por**: resume filas por una o más columnas con agregaciones (suma, conteo, promedio, mín., máx., todas las filas).
- **Pivotar**: convierte los **valores** de una columna en **columnas** (de largo a ancho).
- **Despivotar**: convierte **columnas** en **filas** atributo-valor (de ancho a largo).

Para un modelo de Power BI, la forma que casi siempre quieres es la **larga**: una columna por atributo y una fila por hecho.

### Cómo funciona
```mermaid
flowchart LR
  A["Ancho<br/>Producto | Ene | Feb | Mar"] -- Despivotar --> L["Largo<br/>Producto | Mes | Monto"]
  L -- Pivotar --> A
  L -- Agrupar por --> R["Resumen<br/>Producto | Total"]
```

**Despivotar columnas** vs **despivotar otras columnas**:
- *Despivotar columnas* (`Table.Unpivot`) toma las columnas **seleccionadas** y las convierte en filas. Si el próximo mes agregan la columna `Abr`, **no** se despivota porque no estaba en la lista.
- *Despivotar otras columnas* (`Table.UnpivotOtherColumns`) toma **todo excepto** las seleccionadas (las columnas fijas como `Producto`). Si aparece `Abr`, entra sola. Por eso es la opción robusta para presupuestos y reportes que crecen.

**Agrupar** con `Table.Group`: lista de columnas de agrupación y lista de agregaciones `{nombre, función, tipo}`.

**Pivotar** con `Table.Pivot`: necesitas la lista de valores que serán columnas y una función de agregación para cuando haya varias filas por celda.

### Ejemplo
Presupuesto capturado en Excel con un mes por columna:

```m
let
    // Producto | Ene | Feb | Mar  ->  Producto | Mes | Monto
    Largo = Table.UnpivotOtherColumns(Presupuesto, {"Producto"}, "Mes", "Monto"),
    Tipos = Table.TransformColumnTypes(Largo, {{"Monto", Currency.Type}})
in
    Tipos
```

Agrupar ventas por categoría con total y número de filas:

```m
Resumen = Table.Group(Ventas, {"Categoria"}, {
    {"TotalImporte", each List.Sum([Importe]), type number},
    {"Filas", each Table.RowCount(_), Int64.Type}
})
```

Volver a ancho, una columna por mes, sumando monto:

```m
Ancho = Table.Pivot(Largo, List.Distinct(Largo[Mes]), "Mes", "Monto", List.Sum)
```

Para tener `Mes` como fecha real y poder relacionar con Calendario, después de despivotar convierte `"Ene"` a número de mes (con una tabla de mapeo o un `Record.Field` sobre `[Ene = 1, Feb = 2, ...]`) y arma `#date(2026, NumMes, 1)`.

### Cuándo usarlo y cuándo no
**¿Cuándo despivotar?** Cuando las columnas representan **valores de un mismo atributo** (meses, años, sucursales, tallas) en lugar de atributos distintos. Señales: encabezados como `2023`, `2024`, `2025` o `Ene…Dic`; columnas que "crecen" con el tiempo; necesidad de filtrar por esa dimensión. En forma larga, una sola medida `SUM(Monto)` funciona con cualquier segmentación de mes, y puedes relacionar con Calendario.

**¿Para qué agrupar si DAX agrega?** DAX agrega en tiempo de consulta, y es lo correcto para el análisis. Agrupar en Power Query sirve para:
- **Cambiar la granularidad** a propósito: traer ventas diarias por producto a nivel mensual por categoría para compararlas con un presupuesto mensual, o reducir 500 millones de filas a 5 millones cuando nadie necesita el detalle.
- **Crear dimensiones** (lista única de clientes a partir de las ventas).
- **Tablas de agregación** para rendimiento.
No agrupes si vas a perder detalle que alguien necesitará filtrar.

**Pivotar** es raro para el modelo; úsalo para preparar tablas que se exportan o para casos en que cada valor sí es un atributo distinto (pares clave-valor de un API: `{"campo":"Color","valor":"Rojo"}`).

### Errores comunes
- **Despivotar columnas** (no *otras*) y perder las columnas nuevas.
- **Dejar el presupuesto ancho** y escribir 12 medidas (`[Meta Ene]`, `[Meta Feb]`...).
- **Agrupar antes de filtrar o limpiar**, sumando filas basura o subtotales del Excel.
- **Pivotar con valores duplicados sin función de agregación**: genera errores en las celdas.
- **Totales incluidos en el origen** ("Total general" como un producto más) que duplican al despivotar.

### Para profundizar
- [Despivotar columnas](https://learn.microsoft.com/es-mx/power-query/unpivot-column)
- [Pivotar columnas](https://learn.microsoft.com/es-mx/power-query/pivot-columns)
- [Agrupar o resumir filas](https://learn.microsoft.com/es-mx/power-query/group-by)

Relacionado: [[Granularidad de una tabla de hechos]], [[Agregaciones]].

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- La tabla `Presupuesto` tiene columnas `Producto, Ene, Feb, Mar`. Pásala a `Producto, Mes, Monto`.
> ```m
> Largo = Table.UnpivotOtherColumns(Presupuesto, {"Producto"}, "Mes", "Monto")
> ```

> [!question]- Agrupa `Ventas` por `Categoria` con total de Importe y número de filas.
> ```m
> G = Table.Group(Ventas, {"Categoria"}, {
>     {"Total", each List.Sum([Importe]), type number},
>     {"Filas", each Table.RowCount(_), Int64.Type}
> })
> ```

> [!question]- Pivota `Largo` para volver a tener una columna por mes, sumando Monto.
> ```m
> Ancho = Table.Pivot(Largo, List.Distinct(Largo[Mes]), "Mes", "Monto", List.Sum)
> ```

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cuándo despivotas columnas?
> Cuando los datos vienen en formato "ancho" (una columna por mes, por ejemplo) y necesitas formato "largo" (Mes, Valor) para modelar y filtrar. Es la transformación más útil con reportes de Excel.

> [!question]- ¿Diferencia entre *Despivotar columnas* y *Despivotar otras columnas*?
> La primera despivota las columnas seleccionadas (lista fija); *otras columnas* despivota todo excepto las seleccionadas, así que si llega un mes nuevo se incluye solo. Suele ser más robusto.

> [!question]- ¿Para qué sirve agrupar en Power Query si luego DAX agrega?
> Para reducir granularidad antes de cargar (menos filas) cuando no se necesita el detalle, o para preparar tablas auxiliares. Si se necesita el detalle, mejor no agrupar y dejar que las medidas agreguen.

## Práctica

- [[Lab 2 - Limpieza y combinación de datos en Power Query]]

