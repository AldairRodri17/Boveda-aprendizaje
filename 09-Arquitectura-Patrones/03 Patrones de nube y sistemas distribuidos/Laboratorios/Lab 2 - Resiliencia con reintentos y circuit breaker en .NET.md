---
proyecto: Conocimiento
tipo: laboratorio
nivel: avanzado
progreso: pendiente
estado: borrador
area: Arquitectura y patrones
herramienta: .NET SDK, Microsoft.Extensions.Http.Resilience (Polly)
tags: [arquitectura-patrones, laboratorio]
---
# Lab 2 - Resiliencia con reintentos y circuit breaker en .NET

Parte de: [[09-Arquitectura-Patrones/03 Patrones de nube y sistemas distribuidos/_Índice Patrones de nube y sistemas distribuidos|Patrones de nube y sistemas distribuidos]]

**Cubre:** [[Circuit breaker y reintentos]] · [[Bulkhead y throttling]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Ver en vivo cómo se comportan reintentos, timeouts y circuit breaker frente a un servicio inestable, y medir la diferencia.

## Pasos
1. Crea dos proyectos web mínimos: `Inestable` (puerto 5100) y `Cliente` (puerto 5200).
2. En `Inestable` expón `GET /datos` que falle con 503 en ~50% de las peticiones y, de vez en cuando, tarde 3 s. Agrega `GET /caido` que siempre devuelva 503.
3. En `Cliente` registra un `HttpClient` tipado hacia `Inestable` **sin** resiliencia y un endpoint `GET /probar?n=50` que haga n llamadas y reporte éxitos, fallos y tiempo total.
4. Ejecuta `/probar?n=50` y anota resultados.
5. Agrega el paquete `Microsoft.Extensions.Http.Resilience` y configura en el `HttpClient` un pipeline con: timeout por intento, reintentos con backoff exponencial y jitter solo para errores transitorios, y circuit breaker.
6. Repite la prueba con `/datos` y luego con `/caido`. Observa en los logs cuándo el circuito se abre y que las llamadas fallan rápido.
7. Agrega un límite de concurrencia (bulkhead) de 5 llamadas simultáneas y prueba con 30 llamadas en paralelo (`Task.WhenAll`).
8. Anota en Notas personales qué pasaría si reintentaras un `POST` de cobro con esta política.

## Criterios de terminado
- [ ] Sin resiliencia, `/probar?n=50` contra `/datos` muestra fallos cercanos al 50%.
- [ ] Con reintentos, la tasa de éxito sube claramente y los logs muestran los reintentos con esperas crecientes.
- [ ] Contra `/caido`, después de algunos fallos el circuito se abre y las siguientes llamadas fallan en milisegundos (sin llegar al servidor).
- [ ] Con bulkhead, nunca hay más de 5 llamadas simultáneas a `Inestable` (verificado con un contador en el servidor).
- [ ] Puedes explicar por qué 400/404 no se reintentan.

## Solución

> [!success]- Servicio inestable
> ```csharp
> var app = WebApplication.Create(args);
> var enVuelo = 0; var maximo = 0;
> app.MapGet("/datos", async () =>
> {
>     var actual = Interlocked.Increment(ref enVuelo);
>     maximo = Math.Max(maximo, actual);
>     try
>     {
>         if (Random.Shared.Next(10) == 0) await Task.Delay(3000);
>         return Random.Shared.Next(2) == 0 ? Results.StatusCode(503) : Results.Ok(new { ok = true });
>     }
>     finally { Interlocked.Decrement(ref enVuelo); }
> });
> app.MapGet("/caido", () => Results.StatusCode(503));
> app.MapGet("/maximo", () => maximo);
> app.Run("http://localhost:5100");
> ```

> [!success]- Pipeline de resiliencia en el cliente
> ```csharp
> builder.Services.AddHttpClient("inestable", c => c.BaseAddress = new("http://localhost:5100"))
>     .AddResilienceHandler("mi-pipeline", p =>
>     {
>         p.AddConcurrencyLimiter(5);                      // bulkhead
>         p.AddRetry(new HttpRetryStrategyOptions
>         {
>             MaxRetryAttempts = 3,
>             BackoffType = DelayBackoffType.Exponential,
>             UseJitter = true,
>             Delay = TimeSpan.FromMilliseconds(200)
>         });                                              // reintenta 5xx, 408, 429 y errores de red
>         p.AddCircuitBreaker(new HttpCircuitBreakerStrategyOptions
>         {
>             FailureRatio = 0.5,
>             MinimumThroughput = 10,
>             SamplingDuration = TimeSpan.FromSeconds(10),
>             BreakDuration = TimeSpan.FromSeconds(15)
>         });
>         p.AddTimeout(TimeSpan.FromSeconds(1));           // timeout por intento
>     });
> ```
> El orden importa: la estrategia agregada primero es la más externa. Aquí el reintento envuelve al breaker y el timeout aplica a cada intento. Alternativa rápida: `.AddStandardResilienceHandler()`, que trae una combinación por defecto. Los nombres exactos de opciones pueden variar entre versiones del paquete (Pendiente de confirmar con la documentación de tu versión).

> [!success]- Respuesta sobre el POST de cobro
> Si el primer intento llegó y el servidor cobró pero la respuesta se perdió (timeout), el reintento cobra de nuevo. Para `POST` no idempotentes: no reintentar, o enviar un `Idempotency-Key` que el servidor respete. Ver [[Idempotencia]].

## Notas personales

_Qué me costó, errores que cometí, dudas._
