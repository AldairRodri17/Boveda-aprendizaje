---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
---
# Listas en M

Parte de: [[Lenguaje M - tipos (listas, registros y tablas)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **lista** en M es una secuencia **ordenada** de valores, escrita entre llaves: `{1, 2, 3}`, `{"Norte", "Sur"}`, incluso mezclada `{1, "a", null, #date(2025, 1, 1)}`. Es el equivalente a un arreglo o `IEnumerable` en C#. Cada columna de una tabla, vista sola, es una lista.

### Cómo funciona
**Construcción**:
- Literal: `{"Ene", "Feb", "Mar"}`.
- **Rango** con `..`: `{1..5}` devuelve `{1, 2, 3, 4, 5}`; también con letras: `{"a".."e"}`. Se pueden combinar: `{1..3, 10}` = `{1, 2, 3, 10}`.
- Desde una tabla: `Ventas[Importe]`.
- Generadas: `List.Numbers(1, 12)`, `List.Dates(#date(2025,1,1), 365, #duration(1,0,0,0))`, `List.Generate(...)`.

**Acceso**:
- Por posición, desde 0: `Lista{0}` es el **primer elemento**. Fuera de rango da error; `Lista{10}?` devuelve `null`.
- `List.First(Lista)` y `List.Last(Lista)` (devuelven `null` si está vacía o el valor por omisión que indiques).

**Funciones más útiles**:

| Función | Qué hace | Ejemplo |
|---|---|---|
| `List.Sum`, `List.Average`, `List.Max` | Agregados (ignoran `null`) | `List.Sum({1, null, 3})` = 4 |
| `List.Count` | Número de elementos | |
| `List.Distinct` | Quita repetidos | |
| `List.Select` | Filtra con condición | `List.Select(L, each _ > 2)` |
| `List.Transform` | Aplica función a cada elemento (*map*) | `List.Transform({1,2}, each _ * 10)` |
| `List.Accumulate` | Reduce a un valor (*fold*) | Suma acumulada |
| `List.Contains` | ¿Está el valor? | Filtros con lista de valores |
| `List.Sort`, `List.Reverse`, `List.Zip`, `List.PositionOf` | Orden y posiciones | |

Operadores: `&` concatena (`{1, 2} & {3}` = `{1, 2, 3}`), `=` compara elemento a elemento.

### Ejemplo
Filtrar `Ventas` a una lista de sucursales que llega como parámetro (se pliega a `WHERE IdSucursal IN (...)` en SQL Server):

```m
let
    SucursalesNorte = {101, 102, 115},
    VentasNorte = Table.SelectRows(Ventas, each List.Contains(SucursalesNorte, [IdSucursal]))
in
    VentasNorte
```

Lista de meses en español para ordenar o validar:

```m
Meses = List.Transform({1..12}, each Date.MonthName(#date(2025, _, 1), "es-MX"))
// {"enero", "febrero", ..., "diciembre"}
```

Primer y último folio vendido:

```m
let
    Folios = List.Sort(Ventas[Folio]),
    Primero = Folios{0},
    Ultimo = List.Last(Folios)
in
    [Primero = Primero, Ultimo = Ultimo]
```

Convertir una lista en tabla para cargarla:

```m
Table.FromList({"Bebidas", "Alimentos"}, Splitter.SplitByNothing(), {"Categoria"})
```

### Cuándo usarlo y cuándo no
- **Úsalas** para parámetros con varios valores, series (fechas, números), validaciones y cálculos sobre columnas (`List.Sum(t[Col])`).
- **Úsalas** para construir argumentos de funciones de tabla (listas de columnas en `Table.SelectColumns`, pares de tipos).
- **No conviertas** una tabla grande a listas para "iterar": pierdes el plegado y suele ser más lento que la función de tabla equivalente.
- Si una lista se usa muchas veces dentro de un `each` (por fila), envuélvela con `List.Buffer` para que no se reevalúe en cada fila.

### Errores comunes
- **Índice desde 1**: `Lista{1}` es el **segundo** elemento.
- **`List.First` vs `{0}`** en listas vacías: `{0}` da error, `List.First` da `null`.
- **Rangos invertidos**: `{5..1}` devuelve lista vacía, no descendente.
- **`List.Contains` con texto** distingue mayúsculas; usa `Comparer.OrdinalIgnoreCase` como tercer argumento si no quieres eso.

### Para profundizar
- [Funciones de lista](https://learn.microsoft.com/es-mx/powerquery-m/list-functions)
- [Valores de lista en la especificación](https://learn.microsoft.com/es-mx/powerquery-m/m-spec-values)
- [List.Buffer](https://learn.microsoft.com/es-mx/powerquery-m/list-buffer)

Relacionado: [[Registros en M]], [[Tablas en M]], [[Lenguaje M - funciones y recursividad]].

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué devuelve `{1..5}`?
> La lista `{1, 2, 3, 4, 5}`.

> [!question]- Menciona tres funciones de listas útiles.
> `List.Sum`, `List.Distinct`, `List.Select`, `List.Transform`, `List.Dates`, `List.Generate`, `List.Contains`.

> [!question]- ¿Cómo accedes al primer elemento?
> `Lista{0}`; con `Lista{0}?` devuelve null en vez de error si está vacía.

