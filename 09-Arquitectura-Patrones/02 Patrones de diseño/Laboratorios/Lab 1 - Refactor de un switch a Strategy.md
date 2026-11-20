---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Arquitectura y patrones
herramienta: .NET SDK, VS Code y xUnit
tags: [arquitectura-patrones, laboratorio]
---
# Lab 1 - Refactor de un switch a Strategy

Parte de: [[09-Arquitectura-Patrones/02 Patrones de diseño/_Índice Patrones de diseño|Patrones de diseño]]

**Cubre:** [[Patrón Strategy]] · [[Patrón Factory]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Convertir un `switch` que elige un algoritmo en estrategias inyectables, con pruebas que demuestren que el comportamiento no cambió.

## Pasos
1. Crea la solución: `dotnet new sln -n Envios`, `dotnet new classlib -n Envios.Core`, `dotnet new xunit -n Envios.Tests` y referencia Core desde Tests.
2. En Core escribe la versión "antes": `CalculadoraEnvio.Calcular(string tipo, decimal pesoKg)` con un `switch` para `estandar` (50 + 10/kg), `express` (120 + 15/kg) y `recoger` (0). Tipo desconocido → `ArgumentException`.
3. Escribe pruebas de caracterización (`[Theory]` con `[InlineData]`) para los tres tipos con 0, 1 y 2.5 kg, y para un tipo inválido. Deben pasar **antes** de refactorizar.
4. Define `ICalculoEnvio { string Tipo; decimal Calcular(decimal pesoKg); }` y crea una clase por tipo.
5. Cambia `CalculadoraEnvio` para recibir `IEnumerable<ICalculoEnvio>` y seleccionar por `Tipo`. Sin `switch`.
6. Agrega un tipo nuevo `mismo-dia` (200 + 20/kg) **sin modificar** `CalculadoraEnvio`.
7. Agrega una prueba que construya el contenedor de DI (`ServiceCollection`) y verifique que se resuelven las cuatro estrategias.
8. Escribe en la nota de este lab (Notas personales) cuándo NO harías este refactor.

## Criterios de terminado
- [ ] Las pruebas de caracterización pasan antes y después del refactor sin cambiarlas.
- [ ] `CalculadoraEnvio` no contiene `switch` ni `if` por tipo.
- [ ] Agregar `mismo-dia` solo requirió una clase nueva y una línea de registro.
- [ ] `express` con 2 kg devuelve 150 y `mismo-dia` con 1 kg devuelve 220.
- [ ] Un tipo inexistente lanza `ArgumentException` con el tipo en el mensaje.

## Solución

> [!success]- Estrategias y selección
> ```csharp
> public interface ICalculoEnvio { string Tipo { get; } decimal Calcular(decimal pesoKg); }
> public sealed class EnvioEstandar : ICalculoEnvio { public string Tipo => "estandar"; public decimal Calcular(decimal p) => 50m + 10m * p; }
> public sealed class EnvioExpress  : ICalculoEnvio { public string Tipo => "express";  public decimal Calcular(decimal p) => 120m + 15m * p; }
> public sealed class EnvioRecoger  : ICalculoEnvio { public string Tipo => "recoger";  public decimal Calcular(decimal p) => 0m; }
> public sealed class EnvioMismoDia : ICalculoEnvio { public string Tipo => "mismo-dia"; public decimal Calcular(decimal p) => 200m + 20m * p; }
>
> public sealed class CalculadoraEnvio(IEnumerable<ICalculoEnvio> estrategias)
> {
>     private readonly Dictionary<string, ICalculoEnvio> _porTipo =
>         estrategias.ToDictionary(e => e.Tipo, StringComparer.OrdinalIgnoreCase);
>     public decimal Calcular(string tipo, decimal pesoKg) =>
>         _porTipo.TryGetValue(tipo, out var e) ? e.Calcular(pesoKg)
>         : throw new ArgumentException($"Tipo de envío no soportado: {tipo}", nameof(tipo));
> }
> ```
> El diccionario es una pequeña **fábrica** por llave; `ToDictionary` también detecta tipos duplicados al arrancar.

> [!success]- Pruebas
> ```csharp
> public class CalculadoraEnvioTests
> {
>     private static CalculadoraEnvio Crear() => new([new EnvioEstandar(), new EnvioExpress(), new EnvioRecoger(), new EnvioMismoDia()]);
>
>     [Theory]
>     [InlineData("estandar", 2.5, 75)]
>     [InlineData("express", 2, 150)]
>     [InlineData("recoger", 10, 0)]
>     [InlineData("mismo-dia", 1, 220)]
>     public void Calcula_por_tipo(string tipo, decimal peso, decimal esperado) =>
>         Assert.Equal(esperado, Crear().Calcular(tipo, peso));
>
>     [Fact]
>     public void Tipo_desconocido_lanza() =>
>         Assert.Contains("dron", Assert.Throws<ArgumentException>(() => Crear().Calcular("dron", 1)).Message);
>
>     [Fact]
>     public void DI_resuelve_todas()
>     {
>         var sp = new ServiceCollection()
>             .AddSingleton<ICalculoEnvio, EnvioEstandar>().AddSingleton<ICalculoEnvio, EnvioExpress>()
>             .AddSingleton<ICalculoEnvio, EnvioRecoger>().AddSingleton<ICalculoEnvio, EnvioMismoDia>()
>             .BuildServiceProvider();
>         Assert.Equal(4, sp.GetServices<ICalculoEnvio>().Count());
>     }
> }
> ```
> Necesitas el paquete `Microsoft.Extensions.DependencyInjection` en el proyecto de pruebas.

> [!success]- Cuándo no hacerlo
> Con 2 casos estables que nunca crecerán, un `switch` expresivo es más claro. Strategy paga cuando las variantes crecen, tienen dependencias propias o se prueban por separado.

## Notas personales

_Qué me costó, errores que cometí, dudas._
