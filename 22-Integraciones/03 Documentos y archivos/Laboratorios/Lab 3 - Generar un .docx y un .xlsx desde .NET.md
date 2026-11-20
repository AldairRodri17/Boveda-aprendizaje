---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Integraciones
herramienta: .NET SDK, DocumentFormat.OpenXml y ClosedXML
tags: [integraciones, laboratorio]
---
# Lab 3 - Generar un .docx y un .xlsx desde .NET

Parte de: [[22-Integraciones/03 Documentos y archivos/_Índice Documentos y archivos|Documentos y archivos]]

**Cubre:** [[Generar documentos Word desde .NET (Open XML)]] · [[Generar Excel desde .NET]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Desde una app de consola .NET, generar un reporte de ventas en Word (título, párrafo y tabla) con el Open XML SDK y la misma información en Excel con ClosedXML, con números y fechas con formato real.

## Pasos

1. Crea una app de consola: `dotnet new console -n Reportes` y agrega `DocumentFormat.OpenXml` y `ClosedXML` con `dotnet add package`.
2. Define una lista de 5 ventas `(Fecha, Cliente, Monto)` en memoria.
3. Con Open XML SDK crea `reporte.docx`: un título con estilo de encabezado, un párrafo con la fecha de generación y una tabla con encabezados y las 5 filas.
4. Con ClosedXML crea `reporte.xlsx`: encabezados en negritas, filas con `Fecha` como fecha y `Monto` como número con formato de moneda, y una fila de total con fórmula `SUM`.
5. Ajusta el ancho de columnas y congela la fila de encabezados.
6. Abre ambos archivos en Word/Excel (o LibreOffice) y comprueba que no hay aviso de archivo dañado.
7. Opcional: genera el .docx desde una plantilla con un texto marcador `{{CLIENTE}}` y reemplázalo.

## Criterios de terminado

- [ ] `reporte.docx` abre sin errores y muestra título, párrafo y tabla de 6 filas (encabezado + 5).
- [ ] En `reporte.xlsx` la columna Monto se puede sumar y ordenar (es numérica, no texto).
- [ ] La celda de total contiene una fórmula `SUM(...)` y su valor coincide con la suma de las 5 ventas.
- [ ] La fila de encabezados queda fija al desplazarse.

## Solución

> [!success]- Word con Open XML SDK
> ```csharp
> using DocumentFormat.OpenXml;
> using DocumentFormat.OpenXml.Packaging;
> using DocumentFormat.OpenXml.Wordprocessing;
>
> var ventas = new[] {
>     (new DateTime(2026,11,2), "Ana", 1250.50m), (new DateTime(2026,11,3), "Luis", 980m),
>     (new DateTime(2026,11,4), "Eva", 432.10m), (new DateTime(2026,11,5), "Iván", 2100m),
>     (new DateTime(2026,11,6), "Sara", 315.75m) };
>
> using (var doc = WordprocessingDocument.Create("reporte.docx", WordprocessingDocumentType.Document))
> {
>     var main = doc.AddMainDocumentPart();
>     main.Document = new Document(new Body());
>     var body = main.Document.Body!;
>     Paragraph P(string t, bool negrita = false)
>     {
>         var run = new Run();
>         if (negrita) run.Append(new RunProperties(new Bold()));
>         run.Append(new Text(t));
>         return new Paragraph(run);
>     }
>     body.Append(P("Reporte de ventas", true));
>     body.Append(P($"Generado: {DateTime.Now:dd/MM/yyyy HH:mm}"));
>     var tabla = new Table(new TableProperties(new TableBorders(
>         new TopBorder { Val = BorderValues.Single, Size = 4 }, new BottomBorder { Val = BorderValues.Single, Size = 4 },
>         new InsideHorizontalBorder { Val = BorderValues.Single, Size = 4 })));
>     TableRow Fila(params string[] c) => new(c.Select(x => new TableCell(P(x))));
>     tabla.Append(Fila("Fecha", "Cliente", "Monto"));
>     foreach (var (f, c, m) in ventas) tabla.Append(Fila(f.ToString("dd/MM/yyyy"), c, m.ToString("C")));
>     body.Append(tabla);
> }
> ```
> Para un título con estilo real "Heading1" hay que agregar la parte de estilos (`StyleDefinitionsPart`); usar una plantilla `.docx` lo simplifica.

> [!success]- Excel con ClosedXML
> ```csharp
> using ClosedXML.Excel;
> using var wb = new XLWorkbook();
> var ws = wb.Worksheets.Add("Ventas");
> ws.Cell(1, 1).Value = "Fecha"; ws.Cell(1, 2).Value = "Cliente"; ws.Cell(1, 3).Value = "Monto";
> ws.Row(1).Style.Font.Bold = true;
> var r = 2;
> foreach (var (f, c, m) in ventas)
> {
>     ws.Cell(r, 1).Value = f; ws.Cell(r, 2).Value = c; ws.Cell(r, 3).Value = m; r++;
> }
> ws.Range(2, 1, r - 1, 1).Style.DateFormat.Format = "dd/mm/yyyy";
> ws.Range(2, 3, r, 3).Style.NumberFormat.Format = "$#,##0.00";
> ws.Cell(r, 2).Value = "Total";
> ws.Cell(r, 3).FormulaA1 = $"SUM(C2:C{r - 1})";
> ws.Columns().AdjustToContents();
> ws.SheetView.FreezeRows(1);
> wb.SaveAs("reporte.xlsx");
> ```
> Total esperado de las ventas de ejemplo: `5078.35`.

## Notas personales

_Qué me costó, errores que cometí, dudas._
