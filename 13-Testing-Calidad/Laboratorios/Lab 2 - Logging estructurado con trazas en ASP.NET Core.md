---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Testing y calidad
herramienta: .NET SDK, OpenTelemetry y .NET Aspire dashboard (o consola)
tags: [testing-calidad, laboratorio]
---
# Lab 2 - Logging estructurado con trazas en ASP.NET Core

Parte de: [[13-Testing-Calidad/_Índice Testing y calidad|Testing y calidad]]

**Cubre:** [[Logging estructurado]] · [[Métricas, trazas y logs]] · [[Sentry]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Instrumentar un API para que cada petición genere logs estructurados con `TraceId`, una traza con spans propios y una métrica de negocio, y verlos correlacionados en un dashboard local.

## Pasos
1. Crea un API mínimo con dos servicios: `Pedidos` llama por HTTP a `Inventario`.
2. Reemplaza cualquier log con interpolación por plantillas estructuradas (`{PedidoId}`) y crea al menos un método con `[LoggerMessage]`.
3. Agrega los paquetes de OpenTelemetry (`OpenTelemetry.Extensions.Hosting`, instrumentación de ASP.NET Core y HttpClient, exportador OTLP) y configura trazas, métricas y logs en ambos servicios.
4. Levanta el dashboard de .NET Aspire en modo independiente con Docker (o usa el exportador de consola si no tienes Docker) y apunta el exportador OTLP a él.
5. Crea un `ActivitySource("Pedidos")` y un span manual `ValidarPedido` con un atributo `pedido.id`.
6. Crea una métrica `pedidos_creados` (contador) con `Meter` y increméntala en cada pedido.
7. Haz 10 peticiones, incluida una que falle en Inventario. Encuentra la traza fallida y, desde ella, sus logs.
8. Opcional: agrega Sentry con su plan gratuito y provoca una excepción no controlada; verifica que llega con el `release` y el entorno.

## Criterios de terminado
- [ ] Ningún log usa interpolación (`$"..."`) en el mensaje; todos usan plantillas con campos.
- [ ] En el dashboard, una traza muestra los spans de `Pedidos` → `Inventario` con el span manual `ValidarPedido`.
- [ ] Los logs de esa petición muestran el mismo `TraceId` que la traza.
- [ ] La métrica `pedidos_creados` aparece y su valor coincide con los pedidos exitosos.
- [ ] La petición fallida tiene su span marcado como error y un log de nivel `Error` con la excepción.
- [ ] Ningún log contiene tokens ni datos de tarjeta.

## Solución

> [!success]- Configuración de OpenTelemetry
> ```csharp
> builder.Services.AddOpenTelemetry()
>     .ConfigureResource(r => r.AddService("pedidos"))
>     .WithTracing(t => t
>         .AddSource("Pedidos")
>         .AddAspNetCoreInstrumentation()
>         .AddHttpClientInstrumentation()
>         .AddOtlpExporter())
>     .WithMetrics(m => m
>         .AddMeter("Pedidos")
>         .AddAspNetCoreInstrumentation()
>         .AddOtlpExporter());
> builder.Logging.AddOpenTelemetry(o => { o.IncludeScopes = true; o.AddOtlpExporter(); });
> ```
> El endpoint OTLP se configura con la variable `OTEL_EXPORTER_OTLP_ENDPOINT` (p. ej. `http://localhost:4317`). El nombre de la imagen del dashboard independiente de Aspire y sus puertos: Pendiente de confirmar en la documentación de Aspire.

> [!success]- Span manual, métrica y logs
> ```csharp
> static readonly ActivitySource Fuente = new("Pedidos");
> static readonly Meter Medidor = new("Pedidos");
> static readonly Counter<long> Creados = Medidor.CreateCounter<long>("pedidos_creados");
>
> app.MapPost("/pedidos", async (CrearPedido cmd, HttpClient inventario, ILogger<Program> log) =>
> {
>     using (var span = Fuente.StartActivity("ValidarPedido"))
>     {
>         span?.SetTag("pedido.id", cmd.Id);
>         if (cmd.Cantidad <= 0) return Results.BadRequest();
>     }
>     var r = await inventario.PostAsJsonAsync("/reservar", cmd);
>     if (!r.IsSuccessStatusCode)
>     {
>         log.LogError("Inventario rechazó el pedido {PedidoId} con {Status}", cmd.Id, (int)r.StatusCode);
>         return Results.Problem(statusCode: 502);
>     }
>     Creados.Add(1);
>     log.PedidoCreado(cmd.Id, cmd.Total);
>     return Results.Created($"/pedidos/{cmd.Id}", cmd);
> });
> ```
> El `TraceId` se agrega a los logs automáticamente cuando hay una `Activity` actual y los logs se exportan con OpenTelemetry.

> [!success]- Por qué se correlaciona
> `HttpClient` propaga el header `traceparent`; `Inventario` continúa la misma traza. Logs y spans comparten `TraceId`, así que desde la traza lenta o fallida llegas a sus logs exactos.

## Notas personales

_Qué me costó, errores que cometí, dudas._
