---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: C# y .NET
tags: [csharp-dotnet]
contenido: completo
---
# HttpClient e IHttpClientFactory

Parte de: [[03-CSharp-DotNet/05 ASP.NET Core/_Índice ASP.NET Core|ASP.NET Core]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`HttpClient` es la clase de .NET para hacer peticiones HTTP salientes: llamar a una pasarela de pago, al API de Azure DevOps, a otra API tuya. `IHttpClientFactory` (paquete `Microsoft.Extensions.Http`, incluido en ASP.NET Core) es la fábrica que **crea y administra** esos clientes correctamente: reutiliza conexiones, rota los handlers para respetar cambios de DNS, centraliza la configuración y permite agregar resiliencia ([[Resiliencia con Polly]]) y logging. Si prefieres una API fluida encima, existe [[Flurl]].

### Cómo funciona
Un `HttpClient` es una fachada sobre un **handler** (`SocketsHttpHandler`) que mantiene un pool de conexiones TCP. De ahí salen los dos errores clásicos:

**1. `new HttpClient()` por petición (con `using`).** Cada instancia abre sus propias conexiones; al desecharla, los sockets cerrados quedan en estado `TIME_WAIT` en el sistema operativo por un rato. Bajo carga, miles de peticiones dejan miles de sockets en espera y se **agotan los puertos** disponibles (*socket exhaustion*): empiezas a ver errores de conexión que no tienen que ver con el servidor remoto.

**2. Un `HttpClient` estático para siempre.** Resuelve lo anterior, pero las conexiones viven indefinidamente y **no ven cambios de DNS**: si el servicio remoto cambia de IP (un despliegue blue-green, un failover de Azure), sigues pegándole a la IP vieja.

**La fábrica** resuelve ambos: mantiene un pool de handlers y los **rota** cada cierto tiempo (por defecto **2 minutos**, `SetHandlerLifetime`), mientras que los `HttpClient` que te entrega son baratos y desechables. Otra solución válida fuera de DI es un `HttpClient` estático con `SocketsHttpHandler { PooledConnectionLifetime = TimeSpan.FromMinutes(2) }`.

```mermaid
flowchart LR
    TC[PasarelaCliente - typed client, transient] --> HC[HttpClient nuevo y ligero]
    HC --> H[Handler del pool - rota cada 2 min]
    H --> R[Resiliencia: reintento, timeout]
    R --> L[Logging]
    L --> S[SocketsHttpHandler - conexiones TCP]
```

Formas de uso:
- **Básico**: `IHttpClientFactory.CreateClient()`.
- **Con nombre**: `AddHttpClient("devops", c => ...)` y `CreateClient("devops")`.
- **Typed client**: una clase que recibe `HttpClient` en su constructor y encapsula las llamadas a un servicio. Es la forma recomendada: tipado, configurable y fácil de reemplazar en pruebas.

### Ejemplo
Typed client para una pasarela, con traducción del status a un resultado:

```csharp
public sealed class PasarelaCliente(HttpClient http, ILogger<PasarelaCliente> log)
{
    public async Task<Result<CargoDto>> CrearCargoAsync(NuevoCargo cargo, CancellationToken ct)
    {
        using var resp = await http.PostAsJsonAsync("v1/cargos", cargo, ct);

        if (resp.IsSuccessStatusCode)
            return Result.Ok((await resp.Content.ReadFromJsonAsync<CargoDto>(ct))!);

        var cuerpo = await resp.Content.ReadAsStringAsync(ct);
        log.LogWarning("Pasarela respondió {Status}: {Cuerpo}", (int)resp.StatusCode, cuerpo);

        return resp.StatusCode switch
        {
            HttpStatusCode.UnprocessableEntity => Result.Fallo<CargoDto>("cargo_rechazado"),
            HttpStatusCode.Unauthorized => Result.Fallo<CargoDto>("pasarela_credenciales"),
            _ => Result.Fallo<CargoDto>("pasarela_no_disponible")
        };
    }
}

builder.Services.AddHttpClient<PasarelaCliente>(c =>
{
    c.BaseAddress = new Uri(builder.Configuration["Pasarela:Url"]!);   // termina en "/"
    c.Timeout = TimeSpan.FromSeconds(10);
    c.DefaultRequestHeaders.Add("Accept", "application/json");
});
```

**Si no revisas `IsSuccessStatusCode`**, un 500 o un 404 de la otra API se trata como éxito: intentas deserializar un cuerpo de error como `CargoDto`, obtienes `null` o una excepción de JSON lejos del origen, y el error real se pierde. Siempre usa `EnsureSuccessStatusCode()` (lanza `HttpRequestException` con el status) o, mejor en tus APIs, traduce el status a un `Result` con tu `codigo`.

### Cuándo usarlo y cuándo no
- **Typed client** para cada servicio externo con varias operaciones.
- **Named client** para algo genérico o configuraciones múltiples del mismo servicio.
- **No** captures un typed client en un singleton: el typed client es transient y su handler no rotaría ([[Captive dependency]]).
- **No** pongas la URL base ni llaves en código: van en configuración y secretos.

### Errores comunes
- **`new HttpClient()` en un `using` por llamada**: socket exhaustion.
- **No revisar el status**: errores remotos tratados como éxito.
- **`BaseAddress` sin `/` final y rutas con `/` inicial**: se descarta parte de la ruta.
- **Sin timeout realista**: el valor por defecto es 100 s; tu API se queda colgada esperando.
- **Leer el cuerpo completo de respuestas grandes** con `ReadAsStringAsync`: usa `HttpCompletionOption.ResponseHeadersRead` y streams.

### Para profundizar
- [IHttpClientFactory en .NET (Microsoft Learn)](https://learn.microsoft.com/dotnet/core/extensions/httpclient-factory)
- [Hacer peticiones HTTP con IHttpClientFactory en ASP.NET Core (Microsoft Learn)](https://learn.microsoft.com/aspnet/core/fundamentals/http-requests)
- [Guía de uso de HttpClient (Microsoft Learn)](https://learn.microsoft.com/dotnet/fundamentals/networking/http/httpclient-guidelines)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Por qué `new HttpClient()` por petición es un problema?
> Cada instancia abre sus conexiones; al desecharlas los sockets quedan en `TIME_WAIT` y bajo carga se agotan los puertos (*socket exhaustion*).

> [!question]- ¿Y por qué un `HttpClient` estático para siempre también?
> No ve cambios de DNS. `IHttpClientFactory` rota los handlers (2 min por defecto); o un `SocketsHttpHandler` con `PooledConnectionLifetime`.

> [!question]- ¿Qué es un *typed client*?
> ```csharp
> builder.Services.AddHttpClient<PasarelaCliente>(c => { c.BaseAddress = new("https://api.pasarela.mx/"); c.Timeout = TimeSpan.FromSeconds(10); });
> ```
> Una clase que recibe `HttpClient` en el constructor.

> [!question]- ¿Qué pasa si no revisas `IsSuccessStatusCode`?
> Un 500 de la otra API se trata como éxito y fallas después al leer el cuerpo. Usa `EnsureSuccessStatusCode()` o traduce el status a un `Result`.

## Práctica

- [[Lab 7 - Cliente HTTP tipado con resiliencia y health checks]]

