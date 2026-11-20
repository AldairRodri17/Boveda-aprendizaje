---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: C# y .NET
herramienta: .NET 10 SDK y archivos .http
tags: [csharp-dotnet, laboratorio]
---
# Lab 7 - Cliente HTTP tipado con resiliencia y health checks

Parte de: [[03-CSharp-DotNet/05 ASP.NET Core/_Índice ASP.NET Core|ASP.NET Core]]

**Cubre:** [[HttpClient e IHttpClientFactory]] · [[Resiliencia con Polly]] · [[Health checks]] · [[Logging con ILogger]] · [[BackgroundService]] · [[Caché en memoria, distribuida y HybridCache]] · [[IMemoryCache]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Consumir una API externa inestable con un typed client, protegerte con reintentos/timeout/circuit breaker, cachear respuestas y exponer el estado en `/salud`.

## Pasos

1. Crea dos proyectos web: `Externa` (simula el proveedor) y `Consumidor`.
2. En `Externa`, `GET /tipo-cambio` regresa `{ "usdMxn": 18.5 }` pero falla con 503 en el 40 % de las llamadas y tarda 3 s en otro 10 %.
3. En `Consumidor`, crea `TipoCambioCliente` como typed client (`AddHttpClient<TipoCambioCliente>`) con `BaseAddress` desde Options.
4. Agrega `Microsoft.Extensions.Http.Resilience` y `.AddStandardResilienceHandler()`; ajusta el timeout por intento a 1 s.
5. Cachea el resultado 30 s con `IMemoryCache.GetOrCreateAsync`.
6. Agrega un health check que llame a `Externa` (con timeout corto) y `MapHealthChecks("/salud")`.
7. Agrega un `BackgroundService` con `PeriodicTimer` que refresque la caché cada 30 s y registre fallas sin detener el host.
8. Llama 50 veces `GET /precio-usd` del consumidor y revisa los logs.

## Criterios de terminado

- [ ] Ninguna de las 50 llamadas al consumidor responde 5xx mientras `Externa` está arriba (los reintentos absorben los 503).
- [ ] Los logs muestran reintentos y al menos un timeout de intento.
- [ ] Con `Externa` apagada, tras varias fallas el circuit breaker se abre (aparece en logs) y las respuestas fallan rápido.
- [ ] `/salud` responde `Healthy` con `Externa` arriba y `Unhealthy` con ella apagada.
- [ ] El `BackgroundService` sigue corriendo después de una falla de `Externa`.
- [ ] No hay ningún `new HttpClient()` en el código.

## Solución

> [!success]- Registro
> ```csharp
> builder.Services.AddMemoryCache();
> builder.Services.AddHttpClient<TipoCambioCliente>((sp, c) =>
>         c.BaseAddress = new Uri(sp.GetRequiredService<IOptions<ExternaOptions>>().Value.Url))
>     .AddStandardResilienceHandler(o => o.AttemptTimeout.Timeout = TimeSpan.FromSeconds(1));
> builder.Services.AddHealthChecks().AddCheck<ExternaHealthCheck>("externa");
> builder.Services.AddHostedService<RefrescoTipoCambio>();
> app.MapHealthChecks("/salud");
> ```
> El handler estándar trae timeout total, reintentos con backoff exponencial y jitter, circuit breaker y timeout por intento.

> [!success]- Servicio en segundo plano
> ```csharp
> public sealed class RefrescoTipoCambio(IServiceScopeFactory f, ILogger<RefrescoTipoCambio> log) : BackgroundService
> {
>     protected override async Task ExecuteAsync(CancellationToken stop)
>     {
>         using var t = new PeriodicTimer(TimeSpan.FromSeconds(30));
>         do
>         {
>             try
>             {
>                 await using var scope = f.CreateAsyncScope();
>                 await scope.ServiceProvider.GetRequiredService<ServicioPrecios>().RefrescarAsync(stop);
>             }
>             catch (Exception ex) when (ex is not OperationCanceledException) { log.LogWarning(ex, "No se pudo refrescar"); }
>         } while (await t.WaitForNextTickAsync(stop));
>     }
> }
> ```

## Notas personales

_Qué me costó, errores que cometí, dudas._
