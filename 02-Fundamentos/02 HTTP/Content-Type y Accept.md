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
# Content-Type y Accept

Parte de: [[02-Fundamentos/02 HTTP/_Índice HTTP|HTTP]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Dos headers que dicen **en qué formato** viajan los datos:

- **`Content-Type`** describe el formato del **body que se está enviando**. Lo manda el cliente en un `POST`/`PUT`/`PATCH` y lo manda el servidor en cada respuesta con cuerpo. Ejemplo: `Content-Type: application/json; charset=utf-8`.
- **`Accept`** lo manda el cliente para decir qué formatos **quiere recibir**, en orden de preferencia. Ejemplo: `Accept: application/json`.

El valor es un **media type** (antes "MIME type") con forma `tipo/subtipo` y parámetros opcionales: `text/html`, `application/json`, `application/problem+json`, `image/png`, `multipart/form-data; boundary=...`.

### Cómo funciona
#### Negociación de contenido
```mermaid
sequenceDiagram
    participant C as Cliente
    participant S as Servidor
    C->>S: GET /reportes/ventas<br/>Accept: text/csv, application/json;q=0.8
    alt puede producir CSV
        S-->>C: 200 OK, Content-Type: text/csv
    else solo JSON
        S-->>C: 200 OK, Content-Type: application/json
    else ningún formato aceptable
        S-->>C: 406 Not Acceptable
    end
```

- El parámetro `q` (de 0 a 1) indica preferencia; sin `q` vale 1.
- `*/*` significa "lo que sea".
- Si el servidor no puede producir **ninguno** de los formatos de `Accept`, la respuesta estándar es **`406 Not Acceptable`**; muchas APIs simplemente ignoran la preferencia y responden en su formato por defecto (JSON).
- Si el cliente **envía** un body en un formato que el endpoint no acepta, la respuesta es **`415 Unsupported Media Type`**.

#### Media types que verás a diario
| Media type | Uso |
|---|---|
| `application/json` | APIs |
| `application/problem+json` | Errores con [[ProblemDetails]] |
| `application/x-www-form-urlencoded` | Formulario HTML normal (`campo=valor&otro=valor`) |
| `multipart/form-data` | Formulario con archivos (ver [[Subida de archivos (multipart)]]) |
| `text/csv`, `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet` | Exportaciones (CSV, Excel) |
| `application/octet-stream` | Binario genérico |
| `text/event-stream` | [[Server-Sent Events (SSE)]] |

El `charset` (por ejemplo `utf-8`) dice cómo se codifica el texto. JSON es UTF-8 por especificación.

### Ejemplo
En Wee.Reporteria un mismo recurso puede salir como JSON o como Excel. En el controller:

```csharp
[ApiController]
[Route("api/reportes")]
public class ReportesController : ControllerBase
{
    private const string Xlsx = "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet";

    [HttpGet("ventas")]
    [Produces("application/json", Xlsx)]
    public async Task<IActionResult> Ventas([FromServices] IObtenerVentas consulta, CancellationToken ct)
    {
        var filas = await consulta.EjecutarAsync(ct);
        var accept = Request.Headers.Accept.ToString();

        if (accept.Contains(Xlsx))
        {
            byte[] archivo = GenerarExcel(filas); // tu generador de Excel
            return File(archivo, Xlsx, "ventas.xlsx");
        }
        return Ok(filas); // JSON por defecto
    }

    [HttpPost("filtros")]
    [Consumes("application/json")] // otro Content-Type => 415
    public IActionResult GuardarFiltro(FiltroDto dto) => NoContent();

    private static byte[] GenerarExcel(IEnumerable<object> filas) => [];
}
```

ASP.NET Core también hace negociación automática con *output formatters*: si registras un formatter para un media type y `Accept` lo pide, lo usa. Con `options.ReturnHttpNotAcceptable = true` en `AddControllers` responde `406` en vez de caer a JSON.

Desde el front:

```ts
await fetch("/api/reportes/filtros", {
  method: "POST",
  headers: { "Content-Type": "application/json", Accept: "application/json" },
  body: JSON.stringify({ nombre: "Ventas del mes" }),
});
```

### Cuándo usarlo y cuándo no
- **Envía siempre `Content-Type` correcto** cuando hay body; sin él, el servidor adivina o rechaza.
- Usa `Accept` cuando un recurso tiene varias representaciones reales (JSON, CSV, PDF). Para casos simples, una ruta distinta (`/ventas.xlsx` o `/ventas/exportacion`) también es válida y más fácil de enlazar desde un `<a>`.
- No inventes media types propios salvo que los vayas a documentar y versionar (por ejemplo `application/vnd.empresa.v2+json`, ver [[Versionado de APIs]]).

### Errores comunes
- **Mandar JSON con `Content-Type: text/plain`** (típico al usar `fetch` sin header): el endpoint responde `415`.
- **Confundir los dos headers**: `Content-Type` en un `GET` sin body no sirve para pedir formato; para eso está `Accept`.
- **Confiar en `Content-Type` para validar archivos subidos**: lo pone el cliente y se falsifica fácil; revisa la firma del archivo.
- **Olvidar el `charset`** en texto no UTF-8 (por ejemplo CSV para Excel en Windows): acentos rotos.
- **Responder errores como `text/html`** en una API: el cliente espera `application/json` o `application/problem+json`.

### Para profundizar
- [MDN: Content-Type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type)
- [MDN: Content negotiation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Content_negotiation)
- [Microsoft Learn: Format response data in ASP.NET Core Web API](https://learn.microsoft.com/en-us/aspnet/core/web-api/advanced/formatting)
- [IANA: Media Types](https://www.iana.org/assignments/media-types/media-types.xhtml)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre `Content-Type` y `Accept`?
> `Content-Type` describe el formato del **body que se envía**; `Accept` dice qué formatos **quiere recibir** el cliente. El servidor elige con negociación de contenido.

> [!question]- ¿Qué pasa si el servidor no puede producir ningún formato de `Accept`?
> Debe responder `406 Not Acceptable` (o ignorar la preferencia y usar su formato por defecto, lo que muchas APIs hacen).

> [!question]- ¿Qué `Content-Type` usan un formulario HTML normal y uno con archivos?
> `application/x-www-form-urlencoded` y `multipart/form-data` respectivamente.

## Práctica

- [[Lab 1 - Peticiones HTTP con curl y archivos .http]]

