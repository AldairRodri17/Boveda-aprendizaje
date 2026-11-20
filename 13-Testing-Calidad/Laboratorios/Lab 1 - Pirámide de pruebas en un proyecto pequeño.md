---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Testing y calidad
herramienta: .NET SDK, xUnit, Testcontainers o LocalDB, Playwright
tags: [testing-calidad, laboratorio]
---
# Lab 1 - Pirámide de pruebas en un proyecto pequeño

Parte de: [[13-Testing-Calidad/_Índice Testing y calidad|Testing y calidad]]

**Cubre:** [[Pirámide de pruebas]] · [[Pruebas unitarias]] · [[Pruebas de integración]] · [[Pruebas end-to-end (Playwright y Cypress)]] · [[Cobertura de pruebas]] · [[Qué probar y qué no]] · [[Diseñar código fácil de probar]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Construir la pirámide completa sobre un proyecto pequeño (API de tareas): unitarias del dominio, integración del API con base real y un par de e2e, y medir tiempos y cobertura.

## Pasos
1. Crea la solución: `Tareas.Domain` (classlib), `Tareas.Api` (webapi), `Tareas.UnitTests`, `Tareas.IntegrationTests` (xUnit).
2. Dominio: `Tarea` con reglas: título obligatorio (≤ 100 caracteres), no se puede completar una tarea ya completada, la fecha límite no puede ser pasada (usa `TimeProvider`).
3. Unitarias: al menos 8 pruebas del dominio (incluye bordes: 100 y 101 caracteres; fecha = hoy).
4. API: `POST /tareas`, `GET /tareas/{id}`, `POST /tareas/{id}/completar` con EF Core y SQL Server (LocalDB o contenedor).
5. Integración: con `WebApplicationFactory<Program>` y base real (Testcontainers con SQL Server o una base LocalDB por ejecución) prueba: crear → 201 + `Location`; obtener → 200; completar dos veces → 409; título vacío → 400.
6. E2E (opcional si tienes UI; si no, hazlo contra Swagger UI o una página mínima): con Playwright, un flujo de crear y ver una tarea.
7. Cobertura: `dotnet test --collect:"XPlat Code Coverage"` y genera el reporte con ReportGenerator.
8. Anota en Notas personales: número de pruebas y tiempo por capa, cobertura de ramas del dominio, y un bug que alguna capa atrapó y otra no.

## Criterios de terminado
- [ ] Hay ≥ 8 unitarias, ≥ 4 de integración y ≥ 1 e2e, y todas pasan con `dotnet test`.
- [ ] Las unitarias corren en menos de 1 s en total y no tocan base ni red.
- [ ] La prueba de integración de crear verifica 201 **y** el header `Location`.
- [ ] Ninguna prueba depende de la hora real (usan `FakeTimeProvider`).
- [ ] El reporte de cobertura muestra ≥ 90% de ramas en `Tareas.Domain`.
- [ ] Registraste el tiempo por capa y la proporción resultante.

## Solución

> [!success]- Dominio y unitarias
> ```csharp
> public sealed class Tarea
> {
>     public Guid Id { get; } = Guid.NewGuid();
>     public string Titulo { get; }
>     public DateOnly? FechaLimite { get; }
>     public bool Completada { get; private set; }
>
>     public Tarea(string titulo, DateOnly? fechaLimite, TimeProvider reloj)
>     {
>         if (string.IsNullOrWhiteSpace(titulo) || titulo.Length > 100)
>             throw new ArgumentException("Título inválido", nameof(titulo));
>         var hoy = DateOnly.FromDateTime(reloj.GetUtcNow().UtcDateTime);
>         if (fechaLimite < hoy) throw new ArgumentException("Fecha pasada", nameof(fechaLimite));
>         Titulo = titulo; FechaLimite = fechaLimite;
>     }
>
>     public void Completar()
>     {
>         if (Completada) throw new InvalidOperationException("Ya completada");
>         Completada = true;
>     }
> }
>
> public class TareaTests
> {
>     private readonly FakeTimeProvider _reloj = new(new DateTimeOffset(2026, 11, 9, 12, 0, 0, TimeSpan.Zero));
>
>     [Theory] [InlineData(100)] public void Titulo_de_100_es_valido(int n) => _ = new Tarea(new string('a', n), null, _reloj);
>     [Fact] public void Titulo_de_101_falla() => Assert.Throws<ArgumentException>(() => new Tarea(new string('a', 101), null, _reloj));
>     [Fact] public void Fecha_hoy_es_valida() => _ = new Tarea("x", new DateOnly(2026, 11, 9), _reloj);
>     [Fact] public void Fecha_ayer_falla() => Assert.Throws<ArgumentException>(() => new Tarea("x", new DateOnly(2026, 11, 8), _reloj));
>     [Fact] public void Completar_dos_veces_falla()
>     {
>         var t = new Tarea("x", null, _reloj); t.Completar();
>         Assert.Throws<InvalidOperationException>(t.Completar);
>     }
> }
> ```

> [!success]- Integración con WebApplicationFactory
> ```csharp
> public class TareasApiTests(ApiFactory factory) : IClassFixture<ApiFactory>
> {
>     private readonly HttpClient _http = factory.CreateClient();
>
>     [Fact]
>     public async Task Crear_devuelve_201_y_Location()
>     {
>         var r = await _http.PostAsJsonAsync("/tareas", new { titulo = "Comprar café" });
>         Assert.Equal(HttpStatusCode.Created, r.StatusCode);
>         Assert.NotNull(r.Headers.Location);
>         var get = await _http.GetAsync(r.Headers.Location);
>         Assert.Equal(HttpStatusCode.OK, get.StatusCode);
>     }
>
>     [Fact]
>     public async Task Completar_dos_veces_devuelve_409()
>     {
>         var r = await _http.PostAsJsonAsync("/tareas", new { titulo = "x" });
>         var url = r.Headers.Location + "/completar";
>         await _http.PostAsync(url, null);
>         Assert.Equal(HttpStatusCode.Conflict, (await _http.PostAsync(url, null)).StatusCode);
>     }
> }
> ```
> `ApiFactory : WebApplicationFactory<Program>` sustituye la cadena de conexión en `ConfigureWebHost` por la del contenedor o LocalDB y aplica migraciones al iniciar. Agrega `public partial class Program;` al API para que la prueba lo vea.

> [!success]- E2E con Playwright para .NET
> ```csharp
> // dotnet add package Microsoft.Playwright.Xunit ; luego instala navegadores con el script playwright.ps1 install
> public class FlujoTareas : PageTest
> {
>     [Fact]
>     public async Task Crear_y_ver_tarea()
>     {
>         await Page.GotoAsync("https://localhost:5001/");
>         await Page.GetByLabel("Título").FillAsync("Leer RFC");
>         await Page.GetByRole(AriaRole.Button, new() { Name = "Agregar" }).ClickAsync();
>         await Expect(Page.GetByText("Leer RFC")).ToBeVisibleAsync();
>     }
> }
> ```
> El nombre exacto del paquete de integración con xUnit puede variar (Pendiente de confirmar); con NUnit es `Microsoft.Playwright.NUnit`.

> [!success]- Proporción esperada
> Algo como 10 unitarias (< 1 s), 4–6 de integración (segundos, más el arranque del contenedor) y 1–2 e2e (decenas de segundos). Las e2e son pocas pero dominan el tiempo: por eso se limitan a flujos críticos.

## Notas personales

_Qué me costó, errores que cometí, dudas._
