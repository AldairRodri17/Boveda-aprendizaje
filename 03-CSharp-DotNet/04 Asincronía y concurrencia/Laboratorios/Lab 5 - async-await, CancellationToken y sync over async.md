---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: C# y .NET
herramienta: .NET 10 SDK, dotnet-counters y un generador de carga (bombardier o k6)
tags: [csharp-dotnet, laboratorio]
contenido: completo
---
# Lab 5 - async-await, CancellationToken y sync over async

Parte de: [[03-CSharp-DotNet/04 Asincronía y concurrencia/_Índice Asincronía y concurrencia|Asincronía y concurrencia]]

**Cubre:** [[Task y async-await]] · [[CancellationToken]] · [[Sync over async y thread pool starvation]] · [[Task.WhenAll y Task.WhenAny]] · [[async void y por qué evitarlo]] · [[Hilos y ThreadPool]] · [[dotnet-counters]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Ver con tus propios ojos el daño de bloquear hilos (sync over async) bajo carga, comprobar la cancelación de punta a punta y medir la diferencia entre llamadas secuenciales y en paralelo.

## Pasos

1. `dotnet new webapi -n Asincronia --use-controllers`. Agrega un controller `PruebasController` con tres acciones:
   - `GET /bien` → `await Task.Delay(500, ct); return Ok();`
   - `GET /mal` → `Task.Delay(500).Wait(); return Ok();`
   - `GET /lento?segundos=10` → un bucle de `Task.Delay(1000, ct)` que registre con `ILogger` cada segundo y capture `OperationCanceledException` para registrar "cancelado".
2. Agrega `GET /dashboard` que llame a dos métodos simulados (300 ms y 500 ms) primero en secuencia y luego con `Task.WhenAll`, regresando ambos tiempos medidos con `Stopwatch`.
3. Ejecuta en Release: `dotnet run -c Release`. En otra terminal: `dotnet-counters monitor -n Asincronia --counters System.Runtime`.
4. Lanza carga a `/bien` (p. ej. `bombardier -c 200 -d 30s http://localhost:5000/bien`) y anota latencia promedio, *ThreadPool Thread Count* y *ThreadPool Queue Length*.
5. Repite contra `/mal` y compara.
6. Abre `/lento` en el navegador y cierra la pestaña a los 3 segundos; revisa el log.
7. Agrega por error un método `async void` que lance excepción y llámalo desde una acción; observa qué pasa con el proceso. Luego corrígelo.

## Criterios de terminado

- [ ] Con `/bien` la latencia se mantiene cerca de 500 ms y el número de hilos casi no cambia.
- [ ] Con `/mal` la latencia sube muy por encima de 500 ms, crece la cola del thread pool y el número de hilos aumenta poco a poco (anoto los valores).
- [ ] Al cerrar la pestaña de `/lento`, el log muestra "cancelado" antes de los 10 segundos.
- [ ] `/dashboard` reporta ≈ 800 ms en secuencia y ≈ 500 ms con `WhenAll`.
- [ ] Puedo explicar por qué la excepción en `async void` no se puede atrapar con `try/catch` en quien llama.

## Solución

> [!success]- Acciones
> ```csharp
> [ApiController, Route("")]
> public sealed class PruebasController(ILogger<PruebasController> log) : ControllerBase
> {
>     [HttpGet("bien")] public async Task<IActionResult> Bien(CancellationToken ct) { await Task.Delay(500, ct); return Ok(); }
>     [HttpGet("mal")]  public IActionResult Mal() { Task.Delay(500).Wait(); return Ok(); }
>
>     [HttpGet("lento")]
>     public async Task<IActionResult> Lento(int segundos, CancellationToken ct)
>     {
>         try
>         {
>             for (var i = 1; i <= segundos; i++) { await Task.Delay(1000, ct); log.LogInformation("Segundo {I}", i); }
>             return Ok();
>         }
>         catch (OperationCanceledException) { log.LogInformation("cancelado"); return new EmptyResult(); }
>     }
>
>     [HttpGet("dashboard")]
>     public async Task<object> Dashboard(CancellationToken ct)
>     {
>         var sw = Stopwatch.StartNew();
>         await Simular(300, ct); await Simular(500, ct);
>         var secuencial = sw.ElapsedMilliseconds;
>         sw.Restart();
>         await Task.WhenAll(Simular(300, ct), Simular(500, ct));
>         return new { secuencial, paralelo = sw.ElapsedMilliseconds };
>     }
>     private static Task Simular(int ms, CancellationToken ct) => Task.Delay(ms, ct);
> }
> ```

> [!success]- Qué deberías observar
> Con `/mal` cada petición ocupa un hilo durante 500 ms sin hacer nada. Con 200 conexiones concurrentes, el pool no tiene hilos libres; el runtime agrega hilos lentamente y las peticiones esperan en cola, así que la latencia crece aunque la CPU esté baja. Con `/bien` el hilo se libera durante la espera y pocos hilos atienden todas las peticiones.
>
> La excepción de un `async void` no queda en ninguna `Task`: se relanza en el contexto y, en ASP.NET Core, termina el proceso. La corrección es regresar `Task` y hacer `await`.

## Notas personales

_Qué me costó, errores que cometí, dudas._
