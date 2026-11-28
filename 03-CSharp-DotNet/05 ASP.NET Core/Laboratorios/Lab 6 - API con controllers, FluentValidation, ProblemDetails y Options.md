---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: C# y .NET
herramienta: .NET 10 SDK, VS Code y archivos .http
tags: [csharp-dotnet, laboratorio]
contenido: completo
---
# Lab 6 - API con controllers, FluentValidation, ProblemDetails y Options

Parte de: [[03-CSharp-DotNet/05 ASP.NET Core/_Índice ASP.NET Core|ASP.NET Core]]

**Cubre:** [[ASP.NET Core Controllers]] · [[FluentValidation]] · [[Filtros]] · [[Model binding y validación]] · [[Manejo global de errores (IExceptionHandler)]] · [[Options pattern]] · [[Configuración y appsettings]] · [[Inyección de dependencias]] · [[Ciclos de vida (Singleton, Scoped, Transient)]] · [[Pipeline y middleware]] · [[Orden de los middleware en ASP.NET Core]] · [[Logging con ILogger]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Armar una API pequeña con tu arquitectura: controllers delgados, slice `Command/Result/Handler`, FluentValidation como filtro, errores en ProblemDetails con `codigo` y Options validadas al arrancar.

## Pasos

1. `dotnet new webapi -n Pedidos.Api --use-controllers`. Agrega `FluentValidation.DependencyInjectionExtensions`.
2. Crea `appsettings.json` con la sección `Pedidos: { "MontoMaximo": 5000, "Moneda": "MXN" }` y `PedidosOptions` registrada con `Bind`, `ValidateDataAnnotations` y `ValidateOnStart` (`MontoMaximo` en rango 1–100000, `Moneda` de 3 letras).
3. Crea el slice `CrearPedido` con `Command(string Cliente, decimal Monto)`, `Result` y `Handler` (guarda en un repositorio en memoria **singleton** y regresa el id). Si el monto rebasa `MontoMaximo`, el handler regresa un error de negocio `monto_excedido` (422).
4. Crea `CrearPedido.Validador` (Cliente obligatorio y ≤ 50 caracteres, Monto > 0).
5. Escribe `ValidacionFiltro : IAsyncActionFilter` que busque `IValidator<T>` para cada argumento y responda 400 ProblemDetails con `codigo: "validacion"` y `errors`.
6. Configura `InvalidModelStateResponseFactory` para JSON mal formado → 400 `codigo: "peticion_invalida"`, y `SuppressImplicitRequiredAttributeForNonNullableReferenceTypes = true`.
7. Escribe `ManejadorErrores : IExceptionHandler` que responda 500 `codigo: "error_interno"` con `traceId`, sin detalles internos.
8. Crea también el slice `ObtenerPedido` (Query + Handler) y `PedidosController` con `POST /api/pedidos` (201 + `Location`) y `GET /api/pedidos/{id:long}` (200 o 404 `codigo: "no_encontrado"`). Agrega un `GET /api/pedidos/explota` que lance una excepción.
9. Prueba todo con un archivo `pedidos.http`.

## Criterios de terminado

- [ ] Con `MontoMaximo: 0` en `appsettings.json` la app **no arranca** y el error nombra la propiedad.
- [ ] `POST` válido responde **201** con header `Location: /api/pedidos/{id}`.
- [ ] `POST` con `Cliente` vacío responde **400** con `codigo: "validacion"` y `errors.Cliente`.
- [ ] `POST` con JSON roto (`{ "Cliente": `) responde **400** con `codigo: "peticion_invalida"`.
- [ ] `POST` con monto mayor al máximo responde **422** con `codigo: "monto_excedido"`.
- [ ] `GET` de un id inexistente responde **404** con `codigo: "no_encontrado"`.
- [ ] `GET /explota` responde **500** con `codigo: "error_interno"` y sin pila de llamadas en el cuerpo.
- [ ] El controller no tiene lógica: cada acción es una línea que llama al handler y traduce el resultado.

## Solución

> [!success]- Program.cs
> ```csharp
> var builder = WebApplication.CreateBuilder(args);
> builder.Services.AddOptions<PedidosOptions>()
>     .Bind(builder.Configuration.GetSection("Pedidos"))
>     .ValidateDataAnnotations()
>     .ValidateOnStart();
> builder.Services.AddControllers(o =>
> {
>     o.Filters.Add<ValidacionFiltro>();
>     o.SuppressImplicitRequiredAttributeForNonNullableReferenceTypes = true;
> });
> builder.Services.Configure<ApiBehaviorOptions>(o => o.InvalidModelStateResponseFactory = ctx =>
>     new BadRequestObjectResult(new ProblemDetails { Status = 400, Title = "Petición inválida", Extensions = { ["codigo"] = "peticion_invalida" } }));
> builder.Services.AddValidatorsFromAssemblyContaining<Program>();
> builder.Services.AddSingleton<RepositorioPedidos>();
> builder.Services.AddScoped<CrearPedido.Handler>();
> builder.Services.AddScoped<ObtenerPedido.Handler>();
> builder.Services.AddProblemDetails();
> builder.Services.AddExceptionHandler<ManejadorErrores>();
>
> var app = builder.Build();
> app.UseExceptionHandler();
> app.MapControllers();
> app.Run();
> ```

> [!success]- Filtro de validación
> ```csharp
> public sealed class ValidacionFiltro(IServiceProvider sp) : IAsyncActionFilter
> {
>     public async Task OnActionExecutionAsync(ActionExecutingContext ctx, ActionExecutionDelegate next)
>     {
>         foreach (var arg in ctx.ActionArguments.Values.OfType<object>())
>         {
>             var tipo = typeof(IValidator<>).MakeGenericType(arg.GetType());
>             if (sp.GetService(tipo) is not IValidator v) continue;
>             var r = await v.ValidateAsync(new ValidationContext<object>(arg), ctx.HttpContext.RequestAborted);
>             if (r.IsValid) continue;
>             ctx.Result = new BadRequestObjectResult(new ValidationProblemDetails(r.ToDictionary())
>                 { Status = 400, Title = "Validación", Extensions = { ["codigo"] = "validacion" } });
>             return;
>         }
>         await next();
>     }
> }
> ```

> [!success]- Controller y manejador de errores
> ```csharp
> [ApiController, Route("api/pedidos")]
> public sealed class PedidosController : ControllerBase
> {
>     [HttpPost]
>     public async Task<IActionResult> Crear(CrearPedido.Command cmd, [FromServices] CrearPedido.Handler h, CancellationToken ct) =>
>         (await h.Handle(cmd, ct)).ToActionResult(this, id => CreatedAtAction(nameof(Obtener), new { id }, new { id }));
>
>     [HttpGet("{id:long}")]
>     public async Task<IActionResult> Obtener(long id, [FromServices] ObtenerPedido.Handler h, CancellationToken ct) =>
>         (await h.Handle(new ObtenerPedido.Query(id), ct)).ToActionResult(this, Ok); // NoEncontrado → 404 "no_encontrado"
> }
>
> public sealed class ManejadorErrores(ILogger<ManejadorErrores> log, IProblemDetailsService pds) : IExceptionHandler
> {
>     public async ValueTask<bool> TryHandleAsync(HttpContext ctx, Exception ex, CancellationToken ct)
>     {
>         log.LogError(ex, "Error no controlado");
>         ctx.Response.StatusCode = 500;
>         return await pds.TryWriteAsync(new() { HttpContext = ctx, Exception = ex,
>             ProblemDetails = { Status = 500, Title = "Error interno", Extensions = { ["codigo"] = "error_interno", ["traceId"] = ctx.TraceIdentifier } } });
>     }
> }
> ```
> `ToActionResult` y `Problema` son métodos de extensión tuyos que traducen `Result` → `ProblemDetails` con `codigo`.

## Notas personales

_Qué me costó, errores que cometí, dudas._
