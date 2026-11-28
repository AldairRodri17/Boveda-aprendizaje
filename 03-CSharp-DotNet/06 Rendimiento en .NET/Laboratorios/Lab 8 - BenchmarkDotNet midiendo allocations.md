---
proyecto: Conocimiento
tipo: laboratorio
nivel: avanzado
progreso: pendiente
estado: borrador
area: C# y .NET
herramienta: .NET 10 SDK y BenchmarkDotNet
tags: [csharp-dotnet, laboratorio]
contenido: completo
---
# Lab 8 - BenchmarkDotNet midiendo allocations

Parte de: [[03-CSharp-DotNet/06 Rendimiento en .NET/_Índice Rendimiento en .NET|Rendimiento en .NET]]

**Cubre:** [[BenchmarkDotNet]] · [[Allocations y modos del GC]] · [[StringBuilder]] · [[Span y Memory]] · [[string e inmutabilidad]] · [[JIT y tiered compilation]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Medir tiempo y memoria de varias formas de resolver el mismo problema y aprender a leer las columnas `Mean`, `Ratio`, `Allocated` y `Gen0`.

## Pasos

1. `dotnet new console -n Bench` y `dotnet add package BenchmarkDotNet`.
2. Clase `ParseoCodigos` con `[MemoryDiagnoser]` y `[Params(10, 1000)] public int N;`. En `[GlobalSetup]` genera `N` líneas `"ABC-1234|2026-11-09|150.75"`.
3. Escribe tres benchmarks que sumen el monto (tercera columna) de todas las líneas:
   - `ConSplit` (línea base): `Split('|')` + `decimal.Parse`.
   - `ConSubstring`: `IndexOf` + `Substring`.
   - `ConSpan`: `AsSpan()`, `LastIndexOf('|')` y `decimal.Parse(span)`.
4. Segundo benchmark: armar un CSV de `N` filas con `+=`, con `StringBuilder` y con `StringBuilder` con capacidad inicial.
5. Ejecuta `dotnet run -c Release` y guarda la tabla en tu nota.
6. Prueba `<ServerGarbageCollector>true</ServerGarbageCollector>` vs `false` en el `.csproj` y compara (opcional).

## Criterios de terminado

- [ ] La tabla muestra las columnas `Mean`, `Ratio`, `Allocated` y `Gen0`.
- [ ] `ConSpan` asigna 0 B (o casi) por operación, frente a varios KB de `ConSplit` con N = 1000.
- [ ] `+=` con N = 1000 es varias veces más lento y asigna mucho más que `StringBuilder`.
- [ ] Todos los métodos regresan el mismo total (lo verifico con una prueba o un `Debug.Assert` en el setup).
- [ ] Puedo explicar por qué BenchmarkDotNet no corre en Debug y por qué el benchmark debe regresar un valor.

## Solución

> [!success]- Benchmarks de parseo
> ```csharp
> [MemoryDiagnoser]
> public class ParseoCodigos
> {
>     [Params(10, 1000)] public int N;
>     private string[] _lineas = [];
>
>     [GlobalSetup] public void Setup() => _lineas = Enumerable.Range(0, N).Select(_ => "ABC-1234|2026-11-09|150.75").ToArray();
>
>     [Benchmark(Baseline = true)]
>     public decimal ConSplit() { decimal t = 0; foreach (var l in _lineas) t += decimal.Parse(l.Split('|')[2], CultureInfo.InvariantCulture); return t; }
>
>     [Benchmark]
>     public decimal ConSubstring() { decimal t = 0; foreach (var l in _lineas) t += decimal.Parse(l.Substring(l.LastIndexOf('|') + 1), CultureInfo.InvariantCulture); return t; }
>
>     [Benchmark]
>     public decimal ConSpan() { decimal t = 0; foreach (var l in _lineas) { var s = l.AsSpan(); t += decimal.Parse(s[(s.LastIndexOf('|') + 1)..], CultureInfo.InvariantCulture); } return t; }
> }
> // Program.cs: BenchmarkRunner.Run<ParseoCodigos>();
> ```

> [!success]- Cómo leer el resultado
> - `Split` crea un array y tres strings por línea → más `Allocated` y `Gen0`.
> - `Substring` crea un string por línea.
> - `Span` no crea nada en el heap: `Allocated` ≈ 0 B.
> - Regresar el valor evita que el JIT elimine el trabajo por considerarlo inútil. En Debug no hay optimizaciones, así que los números no representan producción.

## Notas personales

_Qué me costó, errores que cometí, dudas._
