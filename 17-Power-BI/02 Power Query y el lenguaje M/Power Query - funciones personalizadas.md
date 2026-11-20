---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
---
# Power Query - funciones personalizadas

Parte de: [[17-Power-BI/02 Power Query y el lenguaje M/_Índice Power Query y el lenguaje M|Power Query y el lenguaje M]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **función personalizada** en Power Query es una consulta que, en lugar de devolver una tabla, devuelve una **función**: recibe parámetros y produce un resultado. Sirve para aplicar la misma lógica a muchas filas, columnas, archivos o páginas de un API sin copiar pasos. Es el equivalente a extraer un método en C#.

### Cómo funciona
En M una función se escribe con `=>`:

```m
(monto as number, optional tasa as nullable number) as number =>
    Number.Round(monto * (1 + (tasa ?? 0.16)), 2)
```

Formas de crearla:
1. **Desde una consulta**: construyes la transformación para un caso concreto (un archivo), luego clic derecho > **Crear función**. Power Query convierte un parámetro existente en argumento y mantiene la consulta de ejemplo enlazada; si modificas la de ejemplo, la función se actualiza. Es lo que hace *Combinar archivos* automáticamente.
2. **Escribiéndola** en una consulta en blanco con el editor avanzado.

Para usarla: *Agregar columna > Invocar función personalizada* (genera `Table.AddColumn(t, "x", each fn([Col]))`) o directamente en `Table.TransformColumns`, `List.Transform`, etc. Las consultas de función no se cargan al modelo.

### Ejemplo
Monto con IVA de 16 % redondeado:

```m
// Consulta fnIVA
(monto as number) as number => Number.Round(monto * 1.16, 2)
```

Limpiar texto y aplicarlo a `Cliente`:

```m
// Consulta fnLimpiarTexto
(t as nullable text) as nullable text =>
    if t = null then null else Text.Proper(Text.Trim(Text.Clean(t)))

// Uso en la consulta Clientes
Limpia = Table.TransformColumns(Clientes, {{"Cliente", fnLimpiarTexto, type text}})
```

Leer varios CSV con el mismo formato y anexarlos:

```m
// Consulta fnLeerCsv
(ruta as text) as table =>
let
    Csv = Csv.Document(File.Contents(ruta), [Delimiter = ",", Encoding = 65001, QuoteStyle = QuoteStyle.Csv]),
    Encabezados = Table.PromoteHeaders(Csv, [PromoteAllScalars = true])
in
    Encabezados

// Consulta Ventas
let
    Rutas = {"C:\Datos\ventas_2024.csv", "C:\Datos\ventas_2025.csv"},
    Tablas = List.Transform(Rutas, fnLeerCsv),
    Todo = Table.Combine(Tablas)
in
    Todo
```

**Caso típico de uso**: combinar archivos de una carpeta (una función por archivo), paginar un API (una función por página), normalizar textos en varias columnas, o calcular algo que la interfaz no ofrece (por ejemplo, días hábiles entre dos fechas con una lista de festivos).

### Cuándo usarlo y cuándo no
- **Úsala** cuando la misma lógica se repite tres veces o más, o cuando la lógica tiene parámetros naturales (archivo, página, año).
- **Documenta** tipos y significado: agrega metadatos `Documentation.Name` / `Documentation.LongDescription` para que la ayuda aparezca al invocarla.
- **No la uses** si una sola transformación de tabla hace lo mismo (`Table.TransformColumns` con una lambda basta).
- Si varios archivos `.pbix` necesitan la misma función, considera un **dataflow** o copiarla de un repositorio de funciones.

### Errores comunes
- **Invocar una función que llama a un API por cada fila.** Si la tabla tiene 20 000 filas, son 20 000 peticiones HTTP en cada actualización: actualización lentísima, límites de peticiones del API (HTTP 429), posibles bloqueos y nada de plegado. Alternativas: pedir los datos en bloque (endpoint que acepte varios IDs o paginación), traer el catálogo completo una vez y hacer **merge**, o usar `Function.InvokeAfter` para espaciar llamadas si no hay otra salida. Además, el Service puede considerar el origen dinámico y no permitir la actualización si construyes la URL completa en cada fila.
- **No tipar argumentos**: errores crípticos cuando llega `null` o texto.
- **Modificar la consulta de ejemplo** sin darte cuenta de que cambia la función.
- **Funciones que dependen de rutas locales**.

### Para profundizar
- [Uso de funciones personalizadas](https://learn.microsoft.com/es-mx/power-query/custom-function)
- [Funciones en la especificación de M](https://learn.microsoft.com/es-mx/powerquery-m/m-spec-functions)
- [Combinar archivos: información general](https://learn.microsoft.com/es-mx/power-query/combine-files-overview)

Relacionado: [[Lenguaje M - funciones y recursividad]], [[Power Query - consumir un API]].

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Escribe una función `fnIVA` que reciba un número y devuelva el monto con 16 % de IVA redondeado a 2 decimales.
> ```m
> let
>     fnIVA = (monto as number) as number => Number.Round(monto * 1.16, 2)
> in
>     fnIVA
> ```

> [!question]- Escribe una función `fnLimpiarTexto` que haga Trim, Clean y Proper, y aplícala a la columna `Cliente`.
> ```m
> fnLimpiarTexto = (t as nullable text) as nullable text =>
>     if t = null then null else Text.Proper(Text.Clean(Text.Trim(t)))
>
> // uso
> Table.TransformColumns(Origen, {{"Cliente", fnLimpiarTexto, type text}})
> ```

> [!question]- Escribe `fnLeerCsv(ruta as text)` que lea un CSV UTF-8, promueva encabezados y devuélvalo; úsala sobre una lista de dos rutas y anexa.
> ```m
> fnLeerCsv = (ruta as text) as table =>
>     Table.PromoteHeaders(Csv.Document(File.Contents(ruta), [Delimiter = ",", Encoding = 65001]), [PromoteAllScalars = true])
>
> // uso
> Todas = Table.Combine(List.Transform({"C:\datos\a.csv", "C:\datos\b.csv"}, fnLeerCsv))
> ```

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo se convierte una consulta en función?
> Clic derecho → *Crear función* sobre una consulta que usa parámetros, o escribiendo `(param as tipo) => expresión` en el editor avanzado.

> [!question]- Caso típico de uso.
> Aplicar la misma lógica a cada archivo de una carpeta o llamar a un API por cada ID/página con *Invocar función personalizada* (agrega una columna con el resultado).

> [!question]- ¿Qué riesgo tiene invocar una función que llama a un API por cada fila?
> Muchas peticiones: lento, límites de tasa del API y problemas de actualización en el Service. Conviene paginar, filtrar antes y usar `RelativePath`/`Query` en `Web.Contents`.

## Práctica

- [[Lab 3 - Consumir un API en Power Query]]

