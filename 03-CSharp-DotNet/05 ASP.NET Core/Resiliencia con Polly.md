---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: C# y .NET
tags: [csharp-dotnet]
---
# Resiliencia con Polly

Parte de: [[03-CSharp-DotNet/05 ASP.NET Core/_Índice ASP.NET Core|ASP.NET Core]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Polly** es la biblioteca de .NET para **resiliencia**: hacer que tu código tolere fallas transitorias de lo que llama (APIs externas, SQL, colas) con estrategias como reintentos, circuit breaker y timeouts. Desde la versión 8 su núcleo es `Polly.Core` con el concepto de **resilience pipeline**. Microsoft construyó encima `Microsoft.Extensions.Http.Resilience`, que integra Polly con [[HttpClient e IHttpClientFactory]] y trae un *handler estándar* listo.

### Cómo funciona
Estrategias principales:
- **Retry**: reintenta tras una falla transitoria, con espera fija, lineal o **exponencial con jitter** (aleatoriedad para que mil clientes no reintenten al mismo tiempo).
- **Circuit breaker**: si la proporción de fallas supera un umbral, el circuito se **abre** y las llamadas fallan de inmediato durante un tiempo, sin saturar al servicio caído. Pasado ese tiempo pasa a **half-open**: deja pasar una prueba; si funciona se **cierra**, si falla vuelve a abrirse.
- **Timeout**: corta un intento o la operación completa si tarda demasiado.
- **Fallback**: regresa un valor alterno cuando todo falla.
- **Hedging**: lanza una petición paralela si la primera tarda, y usa la que responda antes.
- **Rate limiter / concurrencia**: limita cuántas llamadas simultáneas salen.

Se combinan en un **pipeline**, donde el orden importa: la estrategia de afuera envuelve a las de adentro.

```mermaid
flowchart LR
    A[Llamada] --> RL[Rate limiter]
    RL --> TT[Timeout total 30 s]
    TT --> RT[Retry: 3, exponencial, jitter]
    RT --> CB[Circuit breaker]
    CB --> AT[Timeout por intento 10 s]
    AT --> H[HttpClient]
```

Ese es el orden del **handler estándar** (`AddStandardResilienceHandler`), con sus valores por defecto: limitador de concurrencia, timeout total de 30 s, hasta 3 reintentos exponenciales con jitter (base 2 s), circuit breaker (abre con 10 % de fallas en una ventana de 30 s con al menos 100 llamadas, por 5 s) y timeout por intento de 10 s. Se consideran transitorios los status 5xx, 408 y 429, `HttpRequestException` y los timeouts.

### Ejemplo
Typed client con el handler estándar ajustado:

```csharp
builder.Services.AddHttpClient<PasarelaCliente>(c => c.BaseAddress = new Uri(builder.Configuration["Pasarela:Url"]!))
    .AddStandardResilienceHandler(o =>
    {
        o.Retry.DisableForUnsafeHttpMethods();             // no reintentar POST, PUT, PATCH, DELETE
        o.AttemptTimeout.Timeout = TimeSpan.FromSeconds(5);
        o.TotalRequestTimeout.Timeout = TimeSpan.FromSeconds(20);
        o.CircuitBreaker.SamplingDuration = TimeSpan.FromSeconds(30); // debe ser al menos el doble del timeout por intento
    });
```

Pipeline de Polly para algo que no es HTTP (por ejemplo, un *deadlock* transitorio de SQL Server, error 1205):

```csharp
builder.Services.AddResiliencePipeline("sql", p => p
    .AddRetry(new RetryStrategyOptions
    {
        ShouldHandle = new PredicateBuilder().Handle<SqlException>(e => e.Number == 1205),
        MaxRetryAttempts = 3,
        BackoffType = DelayBackoffType.Exponential,
        UseJitter = true,
        Delay = TimeSpan.FromMilliseconds(200)
    })
    .AddTimeout(TimeSpan.FromSeconds(15)));

// En un handler: ResiliencePipelineProvider<string> prov
// await prov.GetPipeline("sql").ExecuteAsync(async ct => await consulta.Ejecutar(cmd, ct), ct);
```

**¿Por qué no reintentar un POST de cobro a ciegas?** Porque un timeout no significa que el cobro falló: la pasarela pudo haberlo procesado y la respuesta se perdió en el camino. Si reintentas, **cobras dos veces**. Reintenta solo operaciones **idempotentes** (GET, PUT bien diseñado, DELETE) o manda una **llave de idempotencia** (`Idempotency-Key`) que la pasarela respete para detectar el duplicado ([[Idempotencia]]).

**Licencia y costo**: el código de Polly sigue siendo BSD-3 y gratuito. Su repositorio anuncia que desde el 16 de noviembre de 2026 participa en el *Open Source Maintenance Fee*: empresas con ingresos de al menos 20,000 USD de un producto que use Polly pagan 20 USD al mes; individuos, estudiantes, ONG y organizaciones bajo ese umbral no pagan. Qué versiones aplican y si el uso transitivo (vía `Microsoft.Extensions.Http.Resilience`) queda exento: (Pendiente de confirmar) en el anuncio oficial antes de usarlo con un cliente.

### Cuándo usarlo y cuándo no
- **Sí** en todo cliente HTTP hacia servicios externos o internos por red.
- **Sí** para errores transitorios conocidos de SQL o Azure (aunque los SDK de Azure ya traen reintentos propios).
- **No** reintentes errores permanentes (400, 401, 404, validación): solo gastan tiempo.
- **No** apiles reintentos en varias capas (cliente, servicio y gateway): 3 × 3 × 3 = 27 intentos.

### Errores comunes
- **Reintentar POST no idempotentes**.
- **Timeout total menor que la suma de intentos**: los reintentos nunca ocurren.
- **Reintentos sin jitter**: tormentas sincronizadas contra un servicio que intenta recuperarse.
- **Circuit breaker por instancia sin entenderlo**: cada instancia de tu API tiene el suyo.
- **Ocultar la falla final**: después de reintentar, regresa un error claro (`pasarela_no_disponible`).

### Para profundizar
- [Resiliencia HTTP en .NET (Microsoft Learn)](https://learn.microsoft.com/dotnet/core/resilience/http-resilience)
- [Documentación de Polly](https://www.pollydocs.org/)
- [Repositorio de Polly y aviso de OSMF (GitHub)](https://github.com/App-vNext/Polly)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué estrategias ofrece Polly?
> Reintento, circuit breaker, timeout, fallback, hedging y limitador de concurrencia, combinables en un *pipeline*.

> [!question]- ¿Cómo se agrega a un HttpClient en .NET moderno?
> Con `Microsoft.Extensions.Http.Resilience`: `builder.Services.AddHttpClient<PasarelaCliente>().AddStandardResilienceHandler();` (timeouts, reintentos con backoff y circuit breaker).

> [!question]- ¿Por qué no reintentar un POST de cobro a ciegas?
> Puede cobrar dos veces. Reintenta solo operaciones idempotentes, o manda una llave de idempotencia que la pasarela respete.

> [!question]- ¿Qué hace un circuit breaker?
> Tras muchas fallas "abre" y falla de inmediato por un tiempo, sin saturar al servicio caído; luego prueba (*half-open*).

## Práctica

- [[Lab 7 - Cliente HTTP tipado con resiliencia y health checks]]

