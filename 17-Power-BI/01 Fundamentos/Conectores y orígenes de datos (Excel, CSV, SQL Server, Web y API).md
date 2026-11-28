---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
contenido: completo
---
# Conectores y orígenes de datos (Excel, CSV, SQL Server, Web y API)

Parte de: [[17-Power-BI/01 Fundamentos/_Índice Fundamentos|Fundamentos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **conector** es el componente de Power Query que sabe hablar con un tipo de origen: abre el archivo o la conexión, autentica y devuelve tablas. Power BI trae cientos (Excel, CSV, carpeta, SQL Server, SharePoint, Web, OData, Fabric, Dataverse, Salesforce…). Elegir el conector correcto define si la actualización será automática, si habrá plegado de consultas y si necesitarás puerta de enlace.

### Cómo funciona
Cada conector genera un paso `Origen` en M con una función de acceso:

| Origen | Función M típica | Notas |
|---|---|---|
| Libro de Excel | `Excel.Workbook(File.Contents(ruta), null, true)` | Devuelve hojas, **tablas** y rangos con nombre |
| CSV / texto | `Csv.Document(File.Contents(ruta), [Delimiter=",", Encoding=65001])` | 65001 = UTF-8 |
| Carpeta | `Folder.Files(ruta)` | Lista de archivos con su contenido binario |
| SQL Server | `Sql.Database("servidor", "base")` | Soporta Import y DirectQuery, y plegado |
| Web / API | `Web.Contents(url, [RelativePath, Query, Headers])` + `Json.Document` | Para páginas HTML usa `Web.BrowserContents`/`Html.Table` |

Después del origen eliges qué tabla navegar (`Origen{[Item="Ventas", Kind="Table"]}[Data]`) y empiezan las transformaciones.

**Excel: tabla vs hoja.** Una **tabla** de Excel (Insertar > Tabla, con nombre como `tblVentas`) tiene límites definidos y encabezados: crece sola cuando agregan filas y no arrastra celdas sueltas. Una **hoja** devuelve todo el rango usado, incluidos títulos, notas, filas vacías y columnas basura; si alguien escribe algo en la celda Z500, aparece una columna nueva. Siempre que puedas, pide que el origen sea una tabla.

**Carpeta con 12 CSV mensuales.** Usa el conector **Carpeta** (o **SharePoint Folder** si están en SharePoint) y *Combinar archivos*. Power Query crea una consulta de ejemplo y una función que aplica a cada archivo, y luego los anexa. Cuando llegue el CSV del mes 13, basta con soltarlo en la carpeta y actualizar.

**Web vs SQL Server.** Usa **SQL Server** cuando los datos viven en una base: tipos correctos, plegado de filtros, DirectQuery posible y seguridad de base. Usa **Web** para APIs REST/JSON, archivos publicados por URL o tablas HTML; ahí no hay plegado (salvo lo que tú pongas en `Query`) y debes manejar paginación y autenticación.

### Ejemplo
Ventas mensuales que el área comercial deja en `\\fs01\comercial\ventas\` como `ventas_2025_01.csv`, `ventas_2025_02.csv`…:

```m
let
    Origen = Folder.Files("\\fs01\comercial\ventas"),
    SoloCsv = Table.SelectRows(Origen, each Text.Lower([Extension]) = ".csv"
                                   and Text.StartsWith([Name], "ventas_")),
    Leer = Table.AddColumn(SoloCsv, "Datos", each
        Table.PromoteHeaders(
            Csv.Document([Content], [Delimiter = ",", Encoding = 65001, QuoteStyle = QuoteStyle.Csv]),
            [PromoteAllScalars = true])),
    Columnas = Table.SelectColumns(Leer, {"Name", "Datos"}),
    Expandida = Table.ExpandTableColumn(Columnas, "Datos",
        {"Fecha", "IdProducto", "Cantidad", "Importe"}),
    Tipos = Table.TransformColumnTypes(Expandida, {
        {"Fecha", type date}, {"IdProducto", Int64.Type},
        {"Cantidad", Int64.Type}, {"Importe", Currency.Type}}, "es-MX")
in
    Tipos
```

Y el catálogo de productos sale de SQL Server: `Sql.Database("srv-sql01", "Dionisio"){[Schema="dbo", Item="Producto"]}[Data]`.

### Cuándo usarlo y cuándo no
- **Excel/CSV**: datos que mantienen personas o exportaciones; guárdalos en SharePoint/OneDrive para que el Service actualice sin puerta de enlace.
- **Carpeta**: archivos repetitivos con el mismo esquema.
- **SQL Server**: la opción preferida para datos transaccionales. Si es local, necesitas **puerta de enlace** en el Service ([[Puerta de enlace de datos local (gateway)]]).
- **Web/API**: cuando no hay acceso directo a la base; revisa límites de peticiones del API.
- Evita conectores ODBC genéricos si existe uno nativo: el nativo suele plegar mejor.

### Errores comunes
- **Conectar a la hoja** en lugar de la tabla y que aparezcan columnas `Column12`.
- **Rutas locales** (`C:\Users\...`) que en el Service no existen.
- **Codificación equivocada** en CSV (acentos rotos): fija `Encoding = 65001` si es UTF-8.
- **Niveles de privacidad** mal configurados al combinar Web y SQL: el motor puede bloquear la combinación o dejar de plegar. Asigna *Organizacional* a ambos si son internos.
- **Concatenar URL** en `Web.Contents` en vez de usar `RelativePath` y `Query`: el Service no puede validar la actualización.

### Para profundizar
- [Orígenes de datos en Power BI Desktop](https://learn.microsoft.com/es-mx/power-bi/connect-data/desktop-data-sources)
- [Conector de carpeta y combinar archivos](https://learn.microsoft.com/es-mx/power-query/combine-files-overview)
- [Conector de SQL Server](https://learn.microsoft.com/es-mx/power-query/connectors/sql-server)
- [Web.Contents](https://learn.microsoft.com/es-mx/powerquery-m/web-contents)

Relacionado: [[Power Query - consumir un API]], [[Consultas nativas de SQL y vistas para Power BI]], [[Import, DirectQuery, Dual, Live Connection y Direct Lake]].

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- Al conectar a Excel, ¿qué diferencia hay entre elegir una **tabla** y una **hoja**?
> La tabla de Excel (Insertar → Tabla) tiene límites claros y encabezados; la hoja incluye todo el rango usado, con filas vacías o notas. Conviene conectar a tablas con nombre.

> [!question]- ¿Qué conector usas para una carpeta con 12 CSV mensuales del mismo formato?
> El conector **Carpeta** (Folder) y *Combinar archivos*: genera una función de ejemplo y anexa todos. Al llegar un archivo nuevo solo se actualiza.

> [!question]- ¿Cuándo usar el conector Web y cuándo SQL Server?
> Web para URLs/archivos en línea o APIs HTTP (devuelven JSON, CSV, HTML). SQL Server para bases relacionales: permite plegado de consultas y elegir Import o DirectQuery.

## Práctica

- [[Lab 1 - Primer informe de Power BI desde Excel o CSV]]

