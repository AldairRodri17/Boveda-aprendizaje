---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: C# y .NET
herramienta: .NET 10 SDK, xUnit y Microsoft.AspNetCore.Mvc.Testing
tags: [csharp-dotnet, laboratorio]
contenido: completo
---
# Lab 10 - Pruebas con xUnit y WebApplicationFactory

Parte de: [[03-CSharp-DotNet/08 Pruebas en .NET/_Índice Pruebas en .NET|Pruebas en .NET]]

**Cubre:** [[xUnit]] · [[Pruebas de integración con WebApplicationFactory]] · [[Mocks, stubs y fakes]] · [[TimeProvider para probar fechas]] · [[Pruebas de reglas de arquitectura (NetArchTest)]] · [[Moq y NSubstitute]] · [[Testcontainers]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Cubrir la API del Lab 6 con pruebas unitarias del handler, pruebas de integración HTTP de punta a punta y una regla de arquitectura.

## Pasos

1. Crea `Pedidos.Tests` (xunit) y agrega `Microsoft.AspNetCore.Mvc.Testing`, `Microsoft.Extensions.TimeProvider.Testing` y `NetArchTest.Rules`. Referencia `Pedidos.Api`.
2. Haz que el handler reciba `TimeProvider` y rechace pedidos creados en domingo con `codigo: "fuera_de_horario"`.
3. **Unitarias:** prueba el handler con un repositorio *fake* en memoria y `FakeTimeProvider`. Usa `[Theory]` para varios montos.
4. **Integración:** crea `ApiFactory : WebApplicationFactory<Program>` que reemplace `TimeProvider` por un `FakeTimeProvider` fijado en lunes y use `UseEnvironment("Testing")`.
5. Prueba por HTTP: 201 con `Location`, 400 `validacion`, 400 `peticion_invalida`, 404 `no_encontrado` y 500 `error_interno` (lee el JSON y compara `codigo`).
6. Prueba que la app **no arranca** con una configuración inválida (`MontoMaximo = 0` vía `UseSetting`).
7. **Arquitectura:** los controllers no dependen del namespace del repositorio y todas las clases de `Handler` son `sealed`.
8. (Opcional) Reemplaza el repositorio en memoria por SQL Server con Testcontainers y corre las mismas pruebas.

## Criterios de terminado

- [ ] `dotnet test` corre todas las pruebas en verde.
- [ ] La prueba del domingo pasa sin depender del día real (cambiar el reloj del sistema no la afecta).
- [ ] Las pruebas de integración verifican el status **y** el `codigo` del ProblemDetails.
- [ ] Existe una prueba que confirma que una configuración inválida impide arrancar.
- [ ] La regla de arquitectura falla si agrego a un controller una dependencia directa al repositorio (lo pruebo y lo revierto).
- [ ] Ninguna prueba depende del orden de ejecución.

## Solución

> [!success]- Factory y prueba de integración
> ```csharp
> public sealed class ApiFactory : WebApplicationFactory<Program>
> {
>     public FakeTimeProvider Reloj { get; } = new(new DateTimeOffset(2026, 11, 9, 10, 0, 0, TimeSpan.FromHours(-6))); // lunes
>     protected override void ConfigureWebHost(IWebHostBuilder b)
>     {
>         b.UseEnvironment("Testing");
>         b.ConfigureTestServices(s => { s.RemoveAll<TimeProvider>(); s.AddSingleton<TimeProvider>(Reloj); });
>     }
> }
>
> public sealed class PedidosApiTests(ApiFactory f) : IClassFixture<ApiFactory>
> {
>     [Fact]
>     public async Task Cliente_vacio_regresa_400_validacion()
>     {
>         var r = await f.CreateClient().PostAsJsonAsync("/api/pedidos", new { cliente = "", monto = 10 });
>         Assert.Equal(HttpStatusCode.BadRequest, r.StatusCode);
>         var pd = await r.Content.ReadFromJsonAsync<JsonElement>();
>         Assert.Equal("validacion", pd.GetProperty("codigo").GetString());
>     }
> }
> ```
> Si el compilador no encuentra `Program`, agrega `public partial class Program { }` al final de `Program.cs`.

> [!success]- Configuración inválida
> ```csharp
> [Fact]
> public void Config_invalida_impide_arrancar()
> {
>     using var f = new WebApplicationFactory<Program>().WithWebHostBuilder(b => b.UseSetting("Pedidos:MontoMaximo", "0"));
>     var ex = Assert.ThrowsAny<Exception>(() => f.CreateClient());
>     Assert.Contains("MontoMaximo", ex.ToString());
> }
> ```
> `ValidateOnStart` lanza `OptionsValidationException` al iniciar el host.

> [!success]- Regla de arquitectura
> ```csharp
> [Fact]
> public void Controllers_no_dependen_del_repositorio()
> {
>     var r = Types.InAssembly(typeof(Program).Assembly)
>         .That().HaveNameEndingWith("Controller")
>         .ShouldNot().HaveDependencyOn("Pedidos.Api.Datos")
>         .GetResult();
>     Assert.True(r.IsSuccessful, string.Join(", ", r.FailingTypeNames ?? []));
> }
> ```

## Notas personales

_Qué me costó, errores que cometí, dudas._
