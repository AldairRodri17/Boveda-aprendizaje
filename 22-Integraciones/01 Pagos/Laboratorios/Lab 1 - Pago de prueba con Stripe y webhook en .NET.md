---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Integraciones
herramienta: Cuenta de Stripe en modo de prueba, Stripe CLI y .NET SDK
tags: [integraciones, laboratorio]
contenido: completo
---
# Lab 1 - Pago de prueba con Stripe y webhook en .NET

Parte de: [[22-Integraciones/01 Pagos/_Índice Pagos|Pagos]]

**Cubre:** [[Stripe]] · [[Cargos, intents y estados de un pago]] · [[Pasarelas de pago - conceptos]] · [[Conciliación de pagos]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Crear un PaymentIntent desde una API mínima de ASP.NET Core en modo de prueba, confirmarlo con tarjetas de prueba (incluida una con 3D Secure) y recibir el webhook firmado, procesándolo de forma idempotente.

## Pasos

1. Crea o entra a tu cuenta de Stripe y quédate en **modo de prueba** (o sandbox). Copia la llave `sk_test_…`. Nunca uses llaves `live` en este lab.
2. Guarda la llave con `dotnet user-secrets` (no en `appsettings.json` ni en Git).
3. Crea una Minimal API (`dotnet new web`) y agrega el paquete `Stripe.net`.
4. Endpoint `POST /pagos`: crea un PaymentIntent de $250.00 MXN (en centavos) con `metadata[orden_id]`, `automatic_payment_methods` habilitado e `Idempotency-Key` igual al id de la orden. Devuelve `id`, `status` y `client_secret`.
5. Endpoint `POST /webhooks/stripe`: lee el cuerpo **crudo**, verifica la firma con el `whsec_` y registra el evento. Ignora (responde 200) los `id` de evento ya procesados.
6. Instala Stripe CLI, haz `stripe login` y ejecuta `stripe listen --forward-to localhost:PUERTO/webhooks/stripe`.
7. Confirma el pago desde la CLI o el Dashboard con la tarjeta de prueba de éxito y luego con una que pida 3D Secure (consulta la lista oficial de tarjetas de prueba). Observa los estados.
8. Lanza `stripe trigger payment_intent.payment_failed` y verifica que tu orden no queda como pagada. Repite un evento con `stripe events resend <id>` y comprueba que no se procesa dos veces.

## Criterios de terminado

- [ ] `POST /pagos` devuelve un `id` `pi_…` con estado `requires_payment_method` o `requires_confirmation`.
- [ ] Llamar dos veces `POST /pagos` con la misma orden devuelve el mismo `pi_…` (idempotencia).
- [ ] Al confirmar con la tarjeta de éxito, la consola muestra `payment_intent.succeeded` con firma válida y el `orden_id` de la metadata.
- [ ] Un webhook con firma alterada responde 400.
- [ ] El evento reenviado no se procesa dos veces (se registra como duplicado).
- [ ] La llave secreta no aparece en ningún archivo del repositorio.

## Solución

> [!success]- Program.cs
> ```csharp
> using Stripe;
> var builder = WebApplication.CreateBuilder(args);
> var app = builder.Build();
> StripeConfiguration.ApiKey = app.Configuration["Stripe:SecretKey"];   // user-secrets
> var whsec = app.Configuration["Stripe:WebhookSecret"]!;
> var procesados = new HashSet<string>();   // en producción: tabla con índice único
>
> app.MapPost("/pagos", async (string ordenId) =>
> {
>     var pi = await new PaymentIntentService().CreateAsync(new PaymentIntentCreateOptions
>     {
>         Amount = 25000, Currency = "mxn",
>         AutomaticPaymentMethods = new() { Enabled = true },
>         Metadata = new() { ["orden_id"] = ordenId }
>     }, new RequestOptions { IdempotencyKey = $"orden-{ordenId}" });
>     return Results.Ok(new { pi.Id, pi.Status, pi.ClientSecret });
> });
>
> app.MapPost("/webhooks/stripe", async (HttpRequest req) =>
> {
>     var json = await new StreamReader(req.Body).ReadToEndAsync();   // cuerpo crudo
>     Event ev;
>     try { ev = EventUtility.ConstructEvent(json, req.Headers["Stripe-Signature"], whsec); }
>     catch (StripeException) { return Results.BadRequest(); }
>     if (!procesados.Add(ev.Id)) { Console.WriteLine($"duplicado {ev.Id}"); return Results.Ok(); }
>     if (ev.Data.Object is PaymentIntent pi)
>         Console.WriteLine($"{ev.Type} {pi.Id} orden={pi.Metadata.GetValueOrDefault("orden_id")}");
>     return Results.Ok();
> });
> app.Run();
> ```
> Si `ConstructEvent` se queja de la versión de API, revisa que la versión del SDK y la del endpoint coincidan (o usa el parámetro para no lanzar en desajuste, según tu versión de Stripe.net: Pendiente de confirmar).

> [!success]- Comandos
> ```powershell
> dotnet user-secrets init
> dotnet user-secrets set "Stripe:SecretKey" "sk_test_..."
> stripe login
> stripe listen --forward-to localhost:5000/webhooks/stripe   # copia el whsec_ que imprime
> dotnet user-secrets set "Stripe:WebhookSecret" "whsec_..."
> stripe trigger payment_intent.succeeded
> stripe trigger payment_intent.payment_failed
> ```
> Para confirmar tu propio `pi_…` desde la CLI puedes usar `stripe payment_intents confirm pi_... --payment-method pm_card_visa` (método de prueba). Con 3DS el estado pasa por `requires_action`.

## Notas personales

_Qué me costó, errores que cometí, dudas._
