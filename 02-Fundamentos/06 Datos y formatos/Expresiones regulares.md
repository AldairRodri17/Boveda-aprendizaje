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
# Expresiones regulares

Parte de: [[02-Fundamentos/06 Datos y formatos/_Índice Datos y formatos|Datos y formatos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **expresión regular** (regex) es un patrón que describe un conjunto de cadenas. Sirve para **validar** formatos (código postal, RFC), **buscar y extraer** datos de un texto (fechas, folios) y **reemplazar** con transformaciones (reordenar una fecha). Están en todos los lenguajes, en editores (VS Code), en `grep` y en SQL (con matices). Son muy potentes, pero mal escritas son ilegibles, inexactas o incluso un riesgo de seguridad (ReDoS).

### Cómo funciona
**Piezas básicas:**

| Símbolo | Significa | Ejemplo |
|---|---|---|
| `^` / `$` | Inicio / fin de la cadena (o línea con modo multilínea) | `^\d{5}$` |
| `\d` | Un dígito | `\d\d` → `42` |
| `\w` | Letra, dígito o `_` | `\w+` → `hola_1` |
| `\s` | Espacio en blanco | |
| `.` | Cualquier carácter (salvo salto de línea, por defecto) | |
| `[A-Z]`, `[^0-9]` | Clase: uno de esos / ninguno de esos | `[A-ZÑ&]` |
| `+` | Uno o más | `\d+` |
| `*` | Cero o más | `a*` |
| `?` | Cero o uno; tras un cuantificador, lo hace **perezoso** | `colou?r`, `.*?` |
| `{n}`, `{n,m}` | Exactamente n / entre n y m | `\d{4}`, `\w{2,5}` |
| `( )` | Grupo de captura | `(\d{4})-(\d{2})` |
| `(?<nombre> )` | Grupo con nombre | `(?<anio>\d{4})` |
| `\1` | Referencia al grupo 1 | `(\w+)\s+\1` |
| `\b` | Límite de palabra | `\bel\b` |
| `\|` | Alternativa | `MXN\|USD` |

En .NET y Python, `\d` y `\w` aceptan dígitos y letras **Unicode** por defecto (por ejemplo, dígitos árabes); si solo quieres ASCII, usa `[0-9]` o `RegexOptions.ECMAScript` / `re.ASCII`.

**Codicioso vs perezoso.** Los cuantificadores son **codiciosos**: toman lo más posible y luego retroceden si hace falta. Con `?` se vuelven **perezosos**: toman lo menos posible.

```text
Texto:        <b>a</b><b>c</b>
<b>.*</b>  →  <b>a</b><b>c</b>   (todo)
<b>.*?</b> →  <b>a</b>           (el primero)
```

**Anclar al validar.** `\d{5}` **busca** cinco dígitos en cualquier parte, así que `123456` coincide (contiene `12345`). Para validar un código postal hay que anclar: `^\d{5}$`, o usar `fullmatch` en Python. En .NET, `$` acepta un salto de línea final; para rigor total usa `\z` (`^\d{5}\z`).

**ReDoS** (Regular expression Denial of Service). Los motores con *backtracking* (los de .NET, Python, JavaScript) pueden tardar **tiempo exponencial** con patrones de cuantificadores anidados o alternativas que se traslapan, como `(a+)+$` o `(\w+\s?)*$`, ante entradas maliciosas como `aaaaaaaaaaaaaaaaaaaaaaaa!`. Una sola petición puede dejar un núcleo de CPU al 100 % durante minutos. Mitigaciones:
- Evitar cuantificadores anidados y alternativas ambiguas.
- **Timeout**: `new Regex(patron, opciones, TimeSpan.FromMilliseconds(100))`.
- **`RegexOptions.NonBacktracking`** (desde .NET 7): motor de tiempo lineal, sin referencias hacia atrás.
- Limitar la longitud de la entrada antes de aplicar la regex.

### Ejemplo
Patrones de los ejercicios, verificados en Python:

```python
import re

re.fullmatch(r"\d{5}", "64000") is not None      # True (código postal)
re.fullmatch(r"\d{5}", "123456") is not None     # False

rfc = re.compile(r"^[A-ZÑ&]{4}\d{6}[A-Z0-9]{3}$") # RFC persona física simplificado (no valida la fecha)
bool(rfc.match("GODE561231GR8"))                  # True

re.findall(r"\b\d{4}-\d{2}-\d{2}\b", "Pago 2026-01-15 y 2026-02-01")  # ['2026-01-15', '2026-02-01']
re.sub(r"(\d{2})/(\d{2})/(\d{4})", r"\3-\2-\1", "09/11/2026")         # '2026-11-09'
re.findall(r"\b(\w+)\s+\1\b", "es el el último", flags=re.IGNORECASE)  # ['el']
```

En Itzamna.Dionisio, con FluentValidation y regex generada en tiempo de compilación (más rápida y sin costo de construcción en ejecución):

```csharp
public static partial class Patrones
{
    [GeneratedRegex(@"^\d{5}\z", RegexOptions.CultureInvariant, matchTimeoutMilliseconds: 100)]
    public static partial Regex CodigoPostal();

    [GeneratedRegex(@"^[A-ZÑ&]{4}\d{6}[A-Z0-9]{3}\z", RegexOptions.CultureInvariant, matchTimeoutMilliseconds: 100)]
    public static partial Regex RfcPersonaFisica();
}

public sealed class DireccionValidator : AbstractValidator<DireccionDto>
{
    public DireccionValidator()
    {
        RuleFor(x => x.CodigoPostal).NotEmpty().Matches(Patrones.CodigoPostal())
            .WithMessage("El código postal debe tener 5 dígitos.");
        RuleFor(x => x.Rfc).Must(r => r is null || Patrones.RfcPersonaFisica().IsMatch(r.ToUpperInvariant()));
    }
}
```

La regex solo valida **forma**. Que el código postal exista o que la fecha del RFC sea real requiere otra validación (catálogo de códigos postales, parsear la fecha).

### Cuándo usarlo y cuándo no
- **Sí**: validar formatos simples, extraer patrones de texto, búsquedas y reemplazos en el editor, limpieza de datos.
- **No**: parsear formatos con estructura anidada (HTML, JSON, XML, CSV con comillas); usa un parser. Validar correos al 100 % (usa una regex sencilla y confirma enviando un correo). Lógica compleja que nadie podrá mantener; escribe código.

### Errores comunes
1. **No anclar** al validar. `\d{5}` acepta `123456`.
2. **`.*` codicioso** que se traga más de lo esperado. Usa `.*?` o clases negadas (`[^<]*`).
3. **Patrones vulnerables a ReDoS** sin timeout en entradas de usuarios.
4. **Olvidar escapar** caracteres especiales (`.`, `+`, `(`) o no usar cadenas *verbatim* (`@"..."` en C#, `r"..."` en Python).
5. **Asumir que `\d` es solo 0–9.** En .NET y Python acepta dígitos Unicode; usa `[0-9]` si importa.

### Para profundizar
- [Microsoft Learn: expresiones regulares en .NET](https://learn.microsoft.com/dotnet/standard/base-types/regular-expressions)
- [Microsoft Learn: procedimientos recomendados con expresiones regulares](https://learn.microsoft.com/dotnet/standard/base-types/best-practices-regex)
- [Documentación de Python: re](https://docs.python.org/3/library/re.html)
- [OWASP: Regular expression Denial of Service (ReDoS)](https://owasp.org/www-community/attacks/Regular_expression_Denial_of_Service_-_ReDoS)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Regex para un código postal mexicano (5 dígitos).
> `^\d{5}$`

> [!question]- Regex para un RFC de persona física (4 letras, 6 dígitos de fecha, 3 de homoclave), simplificado.
> `^[A-ZÑ&]{4}\d{6}[A-Z0-9]{3}$` (no valida que la fecha exista).

> [!question]- Extrae todas las fechas `AAAA-MM-DD` de un texto en Python.
> ```python
> import re
> re.findall(r'\b\d{4}-\d{2}-\d{2}\b', 'Pago 2026-01-15 y 2026-02-01')
> # ['2026-01-15', '2026-02-01']
> ```

> [!question]- Cambia fechas `DD/MM/AAAA` a `AAAA-MM-DD` con grupos.
> ```python
> re.sub(r'(\d{2})/(\d{2})/(\d{4})', r'\3-\2-\1', '09/11/2026')  # '2026-11-09'
> ```

> [!question]- Regex que encuentre palabras repetidas consecutivas (`el el`).
> `\b(\w+)\s+\1\b` (con la bandera de ignorar mayúsculas si se quiere).

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué significan `^`, `$`, `\d`, `\w`, `+`, `*`, `?` y `{n,m}`?
> `^`/`$` inicio/fin; `\d` dígito; `\w` letra, dígito o `_`; `+` uno o más; `*` cero o más; `?` cero o uno (o hace perezoso al cuantificador anterior); `{n,m}` entre n y m repeticiones.

> [!question]- ¿Qué diferencia hay entre `.*` y `.*?`?
> `.*` es **codicioso**: toma lo más posible. `.*?` es **perezoso**: lo menos posible. En `<b>a</b><b>c</b>`, `<b>.*</b>` toma todo y `<b>.*?</b>` solo `<b>a</b>`.

> [!question]- ¿Qué es ReDoS?
> Denegación de servicio por backtracking catastrófico: patrones como `(a+)+$` con entradas maliciosas tardan tiempo exponencial. Se mitiga evitando cuantificadores anidados, con timeout (`new Regex(p, opts, TimeSpan.FromMilliseconds(100))`) o `RegexOptions.NonBacktracking` en .NET.

> [!question]- Error común: validar con `\d{5}` un código postal. ¿Qué pasa con `123456`?
> Coincide porque busca en cualquier parte. Hay que anclar: `^\d{5}$` (o `fullmatch` en Python).

## Práctica

- [[Lab 1 - Expresiones regulares y fechas en Python]]

