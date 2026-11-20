---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Fundamentos
tags: [fundamentos]
---
# Internacionalización y localización (es-MX)

Parte de: [[02-Fundamentos/06 Datos y formatos/_Índice Datos y formatos|Datos y formatos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
- **Internacionalización (i18n)**: preparar el software para que **pueda** funcionar en distintos idiomas y regiones sin cambiar el código: textos fuera del código (archivos de recursos), formatos que dependen de la cultura, soporte Unicode, diseño que tolera textos más largos.
- **Localización (l10n)**: **adaptarlo** a una región concreta: traducciones para `es-MX`, moneda MXN, formatos de fecha y número mexicanos, terminología local ("celular", "RFC", "CP").

(Los números vienen de las letras entre la primera y la última: i-18 letras-n, l-10 letras-n.) Primero se internacionaliza, una vez; luego se localiza por cada mercado.

### Cómo funciona
En .NET, la **cultura** (`CultureInfo`) define formatos de números, fechas, moneda, ordenamiento y mayúsculas. Hay dos culturas por hilo:
- `CurrentCulture`: formatos (números, fechas, moneda).
- `CurrentUICulture`: idioma de los textos (recursos).

En JavaScript, la API **`Intl`** (`Intl.NumberFormat`, `Intl.DateTimeFormat`, `Intl.Collator`) hace lo mismo con etiquetas BCP 47 como `es-MX`.

**Formatos de `es-MX` frente a otras culturas:**

| Cultura | Número | Moneda | Fecha corta |
|---|---|---|---|
| `es-MX` | `1,234.56` | `$1,234.56` | `09/11/2026` |
| `es-ES` | `1234,56` | `1234,56 €` | `09/11/2026` |
| `en-US` | `1,234.56` | `$1,234.56` | `11/9/2026` |
| Invariante | `1234.56` | — | `11/09/2026` |

México usa **coma para miles y punto decimal**, igual que Estados Unidos, a diferencia de España y gran parte de Latinoamérica. La salida exacta depende de la versión de los datos de cultura (ICU) del sistema; por ejemplo, España omite el separador de miles en números de 4 dígitos.

**Parsear con la cultura del servidor.** El mismo texto se interpreta distinto según dónde corra:
- `decimal.Parse("1,5")` es **1.5** en `es-ES` y **15** en `en-US` o `es-MX` (la coma es separador de miles).
- `DateTime.Parse("09/11/2026")` es 9 de noviembre en `es-MX` y 11 de septiembre en `en-US`.
Un servidor en Azure suele correr con cultura invariante o `en-US`, y tu laptop con `es-MX`: el error aparece solo en producción. Regla: **datos de máquina** (JSON, CSV, XML, base de datos, logs) con `CultureInfo.InvariantCulture` y formatos ISO 8601 (ver [[ISO 8601]]); **cultura del usuario solo para mostrar** y para leer lo que el usuario escribe en un formulario.

**Mayúsculas y comparación.** Las reglas de mayúsculas dependen de la cultura. El caso famoso es el turco: `"i".ToUpper()` con cultura `tr-TR` da `"İ"` (I con punto), así que `"archivo".ToUpper() == "ARCHIVO"` es `false` en un servidor configurado en turco. Para **identificadores** (claves, nombres de archivo, códigos, comparación de correos) usa comparaciones **ordinales**: `string.Equals(a, b, StringComparison.OrdinalIgnoreCase)`. Para **ordenar texto para personas** usa la cultura: en español, `ñ` va después de `n` y los acentos no deben mandar `Ávila` al final de la lista.

### Ejemplo
Formatear y parsear en Itzamna.Dionisio:

```csharp
var mx = CultureInfo.GetCultureInfo("es-MX");
decimal total = 1234.56m;

Console.WriteLine(total.ToString("C", mx));      // $1,234.56
Console.WriteLine(total.ToString("N2", mx));     // 1,234.56
Console.WriteLine(new DateOnly(2026, 11, 9).ToString("D", mx)); // lunes, 9 de noviembre de 2026

// Datos de máquina: siempre invariante
var desdeCsv = decimal.Parse("1234.56", NumberStyles.Number, CultureInfo.InvariantCulture);

// Comparar identificadores
bool igual = string.Equals("ANA@EJEMPLO.MX", "ana@ejemplo.mx", StringComparison.OrdinalIgnoreCase); // true

// Ordenar nombres para mostrar
var nombres = new[] { "Ávila", "Nava", "Ñúñez", "Zamora" };
Array.Sort(nombres, StringComparer.Create(mx, ignoreCase: true)); // Ávila, Nava, Ñúñez, Zamora
```

Fijar la cultura de la app para respuestas que se muestran a usuarios (por ejemplo correos o reportes de Wee.Reporteria):

```csharp
builder.Services.AddLocalization();
var app = builder.Build();
app.UseRequestLocalization(new RequestLocalizationOptions()
    .SetDefaultCulture("es-MX")
    .AddSupportedCultures("es-MX", "en-US")
    .AddSupportedUICultures("es-MX", "en-US"));
```

En Itzamna.Clio (TypeScript):

```ts
const moneda = new Intl.NumberFormat("es-MX", { style: "currency", currency: "MXN" });
moneda.format(1234.56); // "$1,234.56"

const fecha = new Intl.DateTimeFormat("es-MX", { dateStyle: "long", timeZone: "America/Mexico_City" });
fecha.format(new Date("2026-11-09T18:00:00Z")); // "9 de noviembre de 2026"
```

En contenedores Linux, .NET usa la librería **ICU** para los datos de cultura; las imágenes mínimas pueden traer el modo de **globalización invariante** activado (`DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=true`), y entonces `es-MX` no formatea como esperas. Revisa la imagen base si los formatos "salen en inglés".

### Cuándo usarlo y cuándo no
- **i18n desde el inicio** si hay cualquier posibilidad de otro idioma o país: es barato al principio y caro después.
- **Cultura del usuario**: interfaz, correos, reportes, PDF.
- **Invariante**: APIs, archivos de intercambio, base de datos, logs, nombres de archivo.
- No traduzcas datos de negocio que deben quedar fijos (códigos de producto, claves del SAT).

### Errores comunes
1. **Parsear con la cultura del servidor.** Montos multiplicados por 100 o fechas invertidas en producción.
2. **`ToUpper()`/`ToLower()` para comparar** identificadores. Usa `OrdinalIgnoreCase`.
3. **Concatenar textos traducidos** ("Hola " + nombre + ", tienes " + n + " pedidos"). El orden cambia por idioma; usa plantillas con marcadores y reglas de plural.
4. **Formatear dinero a mano** (`"$" + total`). Pierdes separadores y el formato correcto por cultura.
5. **Imagen de contenedor en modo invariante** sin saberlo. Formatos `es-MX` incorrectos.

### Para profundizar
- [Microsoft Learn: globalización en .NET](https://learn.microsoft.com/dotnet/core/extensions/globalization)
- [Microsoft Learn: procedimientos recomendados para comparar cadenas](https://learn.microsoft.com/dotnet/standard/base-types/best-practices-strings)
- [MDN: Intl.NumberFormat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/NumberFormat)
- [Microsoft Learn: globalización y localización en ASP.NET Core](https://learn.microsoft.com/aspnet/core/fundamentals/localization)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre internacionalización (i18n) y localización (l10n)?
> **i18n**: preparar el software para soportar idiomas y regiones (textos externos, formatos por cultura, Unicode). **l10n**: adaptarlo a una región concreta (traducciones es-MX, moneda MXN, formatos).

> [!question]- ¿Cómo se formatean número y moneda en es-MX?
> Separador de miles `,` y decimal `.`: `1,234.56`; moneda `$1,234.56`. En `es-ES` sería `1234,56 €`. Usa `ToString("C", new CultureInfo("es-MX"))` o `Intl.NumberFormat('es-MX', {style:'currency', currency:'MXN'})`.

> [!question]- Error común: parsear números o fechas con la cultura del servidor. ¿Qué pasa?
> El mismo texto se interpreta distinto según dónde corre (`1,5` es 1.5 en `es-ES` y 15 en `en-US`). Para datos de máquina usa `CultureInfo.InvariantCulture` y formatos ISO; la cultura del usuario solo para mostrar.

> [!question]- ¿Por qué comparar cadenas con `ToUpper()` puede fallar?
> Reglas de mayúsculas dependen de la cultura (el caso de la "i" turca). Para identificadores usa `StringComparison.OrdinalIgnoreCase`; para ordenar textos en español, comparaciones con la cultura `es-MX`.

