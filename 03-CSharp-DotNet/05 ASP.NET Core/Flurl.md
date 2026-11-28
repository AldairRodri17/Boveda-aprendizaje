---
proyecto:
  - ReporteriaAzure
proyectos:
  - "[[ReporteriaAzure]]"
tipo: libreria
nivel: basico
estado: completo
tags:
  - conocimiento
  - backend
  - dotnet
  - http
progreso: pendiente
area: C# y .NET
contenido: completo
---
# Flurl

> Librería .NET para armar URL y hacer llamadas HTTP con una sintaxis encadenada (`"url".WithBasicAuth(...).GetJsonAsync<T>()`). [[ReporteriaAzure]] la usa para hablar con la [[Azure DevOps REST API]].

## Qué es y qué problema resuelve
`HttpClient` pide bastante código para cada llamada: armar la URL, agregar encabezados, revisar el código de estado y deserializar. Flurl (`Flurl.Http`) lo hace en una línea y lanza una excepción tipada si la respuesta no es 2xx.

## Cómo funciona por dentro
- **Extensiones sobre `string`/`Url`**: `WithHeader`, `WithBasicAuth`, `WithOAuthBearerToken`, `WithTimeout`, `AppendQueryParam`…
- **Verbos**: `GetJsonAsync<T>`, `PostJsonAsync`, `GetStringAsync`, `ReceiveJson<T>`.
- **Clientes**: por debajo usa `FlurlClient` con un `HttpClient` cacheado por host (evita agotar sockets). Se configura globalmente o por cliente (`FlurlHttp.Clients`).
- **Excepciones** (v4):
  - `FlurlHttpException`: respuesta no 2xx o fallo de red; trae `StatusCode` (`int?`, `null` si no hubo respuesta).
  - `FlurlHttpTimeoutException`: se pasó el timeout.
  - `FlurlParsingException`: el cuerpo no se pudo deserializar.
- **Pruebas**: `using var http = new HttpTest(); http.RespondWithJson(...)` intercepta las llamadas sin red.

## Cuándo usarla y cuándo no
### Ventajas
- Muy poco código por llamada; buena para integraciones chicas.
- `HttpTest` facilita las pruebas.
### Desventajas
- Se aleja de `IHttpClientFactory`: la resiliencia (Polly), los handlers y la telemetría se configuran de otra forma.
- La llamada estática sobre un `string` es difícil de reemplazar por un falso si no se envuelve en una interfaz.

## Alternativas
| Alternativa | En qué se diferencia |
|---|---|
| `HttpClient` + `IHttpClientFactory` | Lo estándar de .NET; más verboso, con integración nativa de resiliencia |
| Refit | Interfaz con atributos que genera el cliente |
| RestSharp | Otra API fluida, con más historia y otro estilo |

## Cómo se usó en ReporteriaAzure
- Detrás de la interfaz `IAzureDevOpsClient` (Application), así el handler se prueba con un falso y Flurl no sale de Infrastructure.
- Cada llamada usa `WithBasicAuth("", Pat)`, `WithTimeout` (de `Azure:Timeout`) y `WithAutoRedirect(false)`.
- `EjecutarAsync<T>` traduce las excepciones de Flurl a las de Application (cada una con su `codigo`, ver [[ProblemDetails]]).

```csharp
// ReporteriaAzure/Infrastructure/AzureDevOps/AzureDevOpsClient.cs
catch (FlurlHttpTimeoutException ex)
{
    throw new AzureDevOpsNoDisponibleException("Azure DevOps no respondió a tiempo. Intenta de nuevo.", ex);
}
catch (FlurlParsingException ex)
{
    // Típico de un PAT inválido: Azure manda HTML (203) en lugar de JSON.
    throw new AzureDevOpsSinAccesoException(
        "Azure DevOps no aceptó el token (PAT). Revisa que esté vigente y tenga permiso de lectura de Work Items.", ex);
}
```

> [!note] Orden de los `catch`
> `FlurlHttpTimeoutException` y `FlurlParsingException` heredan de `FlurlHttpException`: van **antes** del `catch` general.

## Errores comunes y buenas prácticas
- Dejar que `FlurlHttpException` llegue al usuario como 500: tradúcela en la capa de infraestructura.
- Olvidar el `CancellationToken` (`GetJsonAsync<T>(cancellationToken: ct)`).
- Poner el token en la URL en lugar de un encabezado.

## Qué estudiar después
- `IHttpClientFactory` y resiliencia con Polly.
- [[Inyección de dependencias]], [[Arquitectura hexagonal]] (puertos y adaptadores).

## Documentación oficial
- https://flurl.dev/docs/fluent-http/
- https://flurl.dev/docs/testable-http/

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué aporta Flurl?
> Una API fluida para construir URLs y hacer peticiones: `await url.AppendPathSegment("x").SetQueryParams(new { p = 1 }).GetJsonAsync<T>();`.

> [!question]- ¿Cómo se prueba código con Flurl?
> Con `HttpTest`: `using var http = new HttpTest(); http.RespondWithJson(obj);` y luego `http.ShouldHaveCalled(url)`.

> [!question]- ¿Qué hace por defecto ante un 4xx/5xx?
> Lanza `FlurlHttpException`; se puede permitir con `AllowHttpStatus` o atraparla y leer `ex.StatusCode`.

