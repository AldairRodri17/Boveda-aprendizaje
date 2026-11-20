---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
---
# Power Query - limpieza de datos

Parte de: [[17-Power-BI/02 Power Query y el lenguaje M/_Índice Power Query y el lenguaje M|Power Query y el lenguaje M]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Limpiar datos** es dejar una tabla en forma "rectangular y confiable": una fila por registro, una columna por atributo, encabezados correctos, sin filas basura, textos normalizados y valores faltantes resueltos. Es la tarea más frecuente en Power Query, sobre todo con Excel capturados a mano y exportaciones de sistemas viejos.

### Cómo funciona
Transformaciones de limpieza más comunes y su función M:

| Transformación | Función M | Para qué |
|---|---|---|
| Quitar filas superiores | `Table.Skip` | Títulos y logotipos antes del encabezado |
| Usar primera fila como encabezado | `Table.PromoteHeaders` | Encabezados reales |
| Quitar filas en blanco | `Table.SelectRows` con `List.NonNullCount` | Separadores vacíos |
| Quitar columnas | `Table.RemoveColumns` / `Table.SelectColumns` | Columnas sin uso |
| Recortar / Limpiar | `Text.Trim` / `Text.Clean` | Espacios y caracteres invisibles |
| Mayúsculas / Nombre propio | `Text.Upper` / `Text.Proper` | Normalizar |
| Reemplazar valores | `Table.ReplaceValue` | `"N/A"` → `null` |
| Rellenar hacia abajo | `Table.FillDown` | Celdas combinadas en Excel |
| Quitar duplicados | `Table.Distinct` | Catálogos repetidos |
| Dividir columna | `Table.SplitColumn` | `"Jalisco - Guadalajara"` |

**Recortar vs Limpiar.** *Recortar* (`Text.Trim`) quita espacios al inicio y al final (no los de en medio). *Limpiar* (`Text.Clean`) quita **caracteres de control no imprimibles**: saltos de línea, tabuladores y otros que llegan de copiar y pegar o de exportaciones. Muchas veces necesitas ambos.

**Rellenar hacia abajo.** Los reportes de Excel suelen poner la región solo en la primera fila de cada bloque (o en celdas combinadas). Power Query las ve como `null` en las filas siguientes. *Rellenar > Hacia abajo* copia el último valor no nulo hasta el siguiente.

### Ejemplo
Un Excel exportado de un sistema de cobranza: dos filas de título, encabezados en la tercera, filas vacías entre bloques, región solo al inicio de cada bloque y nombres de cliente con espacios y mayúsculas mezcladas.

```m
let
    Libro = Excel.Workbook(File.Contents(pRutaCobranza), null, true),
    Origen = Libro{[Item = "Reporte", Kind = "Sheet"]}[Data],
    // 1. quitar las 2 filas de título y promover encabezados
    SinTitulo = Table.Skip(Origen, 2),
    Encabezados = Table.PromoteHeaders(SinTitulo, [PromoteAllScalars = true]),
    // 2. quitar filas completamente vacías
    SinVacias = Table.SelectRows(Encabezados, each
        not List.IsEmpty(List.RemoveMatchingItems(Record.FieldValues(_), {"", null}))),
    // 3. rellenar la región hacia abajo y quitar filas sin producto (subtotales)
    Relleno = Table.FillDown(SinVacias, {"Region"}),
    ConProducto = Table.SelectRows(Relleno, each [Producto] <> null),
    // 4. normalizar texto de cliente
    Cliente = Table.TransformColumns(ConProducto, {{"Cliente",
        each Text.Proper(Text.Trim(Text.Clean(_))), type text}}),
    // 5. "N/A" -> null
    SinNA = Table.ReplaceValue(Cliente, "N/A", null, Replacer.ReplaceValue, {"Vendedor"})
in
    SinNA
```

El paso 2 es el que genera la interfaz con *Quitar filas > Quitar filas en blanco*.

### Cuándo usarlo y cuándo no
- **En Power Query** cuando el origen es un archivo o un sistema que no controlas.
- **En el origen (SQL)** cuando puedes corregir el dato de raíz o crear una vista: la limpieza sirve para todos los consumidores, no solo para Power BI.
- **No limpies en DAX** con columnas calculadas lo que puede hacerse aquí.
- Si la misma limpieza se repite en varios informes, conviértela en **función** o en un **dataflow**.

### Errores comunes
- **Quitar duplicados en texto sin normalizar.** `Table.Distinct` en Power Query **distingue mayúsculas y espacios**: `"Acme"`, `"ACME"` y `"Acme "` son tres valores distintos. Pero el modelo (VertiPaq) **no distingue mayúsculas**: al cargar los trata como el mismo valor. Resultado: una dimensión que "no tiene duplicados" en Power Query falla al crear la relación 1:* porque el modelo sí ve duplicados. Normaliza con `Text.Trim` + `Text.Upper` (o usa `Comparer.OrdinalIgnoreCase` en `Table.Distinct`) antes de quitar duplicados.
- **Quitar filas superiores por número** cuando el título cambia de tamaño; mejor filtrar o buscar la fila del encabezado.
- **Rellenar hacia abajo antes de ordenar** o filtrar mal y propagar valores al bloque equivocado.
- **Reemplazar valores en todas las columnas** sin querer.
- **Usar *Quitar errores*** como limpieza: esconde problemas (ver [[Power Query - manejo de errores]]).

### Para profundizar
- [Table.Distinct](https://learn.microsoft.com/es-mx/powerquery-m/table-distinct)
- [Rellenar valores en una columna](https://learn.microsoft.com/es-mx/power-query/fill-values-column)
- [Funciones de texto de M](https://learn.microsoft.com/es-mx/powerquery-m/text-functions)

Practica en [[Lab 2 - Limpieza y combinación de datos en Power Query]].

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Escribe los pasos M para: quitar las 2 primeras filas de `Origen`, promover encabezados y quitar filas completamente vacías.
> ```m
> SinTop = Table.Skip(Origen, 2),
> Encab = Table.PromoteHeaders(SinTop, [PromoteAllScalars = true]),
> SinVacias = Table.SelectRows(Encab, each not List.IsEmpty(
>     List.RemoveMatchingItems(Record.FieldValues(_), {"", null})))
> ```

> [!question]- Normaliza la columna `Cliente`: sin espacios sobrantes y en formato Nombre Propio.
> ```m
> Limpio = Table.TransformColumns(SinVacias,
>     {{"Cliente", each Text.Proper(Text.Trim(_)), type text}})
> ```

> [!question]- Rellena hacia abajo `Region` y luego quita filas donde `Producto` sea null.
> ```m
> Relleno = Table.FillDown(Limpio, {"Region"}),
> Filtro = Table.SelectRows(Relleno, each [Producto] <> null)
> ```

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- Menciona cinco transformaciones de limpieza comunes.
> Quitar filas superiores/vacías, promover encabezados, recortar (Trim) y limpiar (Clean) texto, reemplazar valores, rellenar hacia abajo, quitar duplicados, dividir columnas, cambiar tipos.

> [!question]- ¿Qué diferencia hay entre *Recortar* y *Limpiar*?
> Recortar (`Text.Trim`) quita espacios al inicio y al final. Limpiar (`Text.Clean`) quita caracteres no imprimibles (saltos de línea, tabuladores de control).

> [!question]- Caso: un reporte de Excel tiene la región solo en la primera fila de cada bloque. ¿Qué haces?
> *Rellenar → Hacia abajo* (`Table.FillDown`) sobre la columna Región.

> [!question]- Error común al quitar duplicados en una columna de texto.
> Power Query distingue mayúsculas/minúsculas y espacios: "Ana" y "ana " son distintos. Normaliza (Trim, mayúsculas) antes de quitar duplicados. Ojo: el modelo (VertiPaq) sí es insensible a mayúsculas, lo que puede causar errores de clave duplicada al cargar.

## Práctica

- [[Lab 2 - Limpieza y combinación de datos en Power Query]]

