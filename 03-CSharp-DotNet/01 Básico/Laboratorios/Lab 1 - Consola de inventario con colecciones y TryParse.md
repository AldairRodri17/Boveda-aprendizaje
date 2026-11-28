---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: C# y .NET
herramienta: .NET 10 SDK y VS Code
tags: [csharp-dotnet, laboratorio]
contenido: completo
---
# Lab 1 - Consola de inventario con colecciones y TryParse

Parte de: [[03-CSharp-DotNet/01 Básico/_Índice Básico|Básico]]

**Cubre:** [[Solución, proyecto y estructura de carpetas]] · [[Tipos primitivos]] · [[null y nullable reference types]] · [[Operadores de null en CSharp]] · [[Colecciones (List, Dictionary, HashSet, Queue, Stack)]] · [[Dictionary de TKey y TValue]] · [[HashSet de T]] · [[Enums]] · [[Excepciones (try, catch, finally, throw)]] · [[Conversión de tipos (casting, parse, TryParse)]] · [[DateTime vs DateTimeOffset]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Construir una app de consola que lea movimientos de inventario de un archivo de texto, valide cada línea sin que el programa truene, y muestre existencias por producto.

## Preparación

Crea `movimientos.txt` con este contenido (formato `fecha|codigo|tipo|cantidad|precio`):

```
2026-11-01T09:00:00-06:00|A100|Entrada|10|25.50
2026-11-01T10:30:00-06:00|B200|Entrada|5|100
2026-11-02T08:15:00-06:00|A100|Salida|3|25.50
2026-11-02T09:00:00-06:00|C300|Entrada|abc|10
2026-11-03T12:00:00-06:00|B200|Devolucion|1|100
2026-11-03T13:00:00-06:00|A100|Salida|20|25.50
```

## Pasos

1. Crea la solución y el proyecto: `dotnet new sln -n Inventario`, `dotnet new console -n Inventario.App -o src/Inventario.App`, `dotnet sln add src/Inventario.App`. Activa `<Nullable>enable</Nullable>` y `<TreatWarningsAsErrors>true</TreatWarningsAsErrors>`.
2. Define `enum TipoMovimiento { Entrada, Salida }` y un `record Movimiento(DateTimeOffset Fecha, string Codigo, TipoMovimiento Tipo, int Cantidad, decimal Precio)`.
3. Escribe `static Movimiento? Parsear(string linea, out string? error)` que use solo `TryParse` (con `CultureInfo.InvariantCulture` para el precio) y `Enum.TryParse`. No uses `Parse`.
4. Lee el archivo, acumula existencias en un `Dictionary<string, int>` y guarda en un `HashSet<string>` los códigos con errores.
5. Una salida que deje la existencia negativa no se aplica: lanza una excepción propia `ExistenciaInsuficienteException`, atrápala en el bucle y regístrala como error de esa línea.
6. Imprime existencias, valor del inventario (`cantidad × último precio`) y la lista de errores con número de línea. Muestra las fechas en UTC.

## Criterios de terminado

- [ ] `dotnet build` compila sin warnings con `Nullable` y `TreatWarningsAsErrors` activos.
- [ ] El programa termina sin excepciones no atrapadas aunque haya líneas inválidas.
- [ ] Existencias finales: `A100 = 7`, `B200 = 5`, y `C300` no aparece.
- [ ] Se reportan 3 errores: línea 4 (cantidad inválida), línea 5 (tipo inválido) y línea 6 (existencia insuficiente).
- [ ] La primera fecha se imprime como `2026-11-01 15:00:00Z`.
- [ ] Puedo explicar por qué uso `decimal` para el precio y `DateTimeOffset` para la fecha.

## Solución

> [!success]- Parseo seguro
> ```csharp
> static Movimiento? Parsear(string linea, out string? error)
> {
>     var p = linea.Split('|');
>     error = null;
>     if (p.Length != 5) { error = "columnas"; return null; }
>     if (!DateTimeOffset.TryParse(p[0], CultureInfo.InvariantCulture, DateTimeStyles.None, out var f)) { error = "fecha"; return null; }
>     if (!Enum.TryParse<TipoMovimiento>(p[2], ignoreCase: true, out var t) || !Enum.IsDefined(t)) { error = "tipo"; return null; }
>     if (!int.TryParse(p[3], out var c) || c <= 0) { error = "cantidad"; return null; }
>     if (!decimal.TryParse(p[4], NumberStyles.Number, CultureInfo.InvariantCulture, out var pr)) { error = "precio"; return null; }
>     return new Movimiento(f, p[1], t, c, pr);
> }
> ```

> [!success]- Bucle principal
> ```csharp
> var existencias = new Dictionary<string, int>();
> var precios = new Dictionary<string, decimal>();
> var conError = new HashSet<string>();
> var errores = new List<string>();
> var n = 0;
> foreach (var linea in File.ReadLines("movimientos.txt"))
> {
>     n++;
>     var m = Parsear(linea, out var e);
>     if (m is null) { errores.Add($"Línea {n}: {e}"); conError.Add(linea.Split('|')[1]); continue; }
>     try
>     {
>         var actual = existencias.GetValueOrDefault(m.Codigo);
>         var nueva = m.Tipo == TipoMovimiento.Entrada ? actual + m.Cantidad : actual - m.Cantidad;
>         if (nueva < 0) throw new ExistenciaInsuficienteException(m.Codigo, actual, m.Cantidad);
>         existencias[m.Codigo] = nueva;
>         precios[m.Codigo] = m.Precio;
>         Console.WriteLine($"{m.Fecha.UtcDateTime:u} {m.Codigo} {m.Tipo} {m.Cantidad}");
>     }
>     catch (ExistenciaInsuficienteException ex) { errores.Add($"Línea {n}: {ex.Message}"); }
> }
> var valor = existencias.Sum(x => x.Value * precios[x.Key]);
> ```
> Valor esperado del inventario: `7 × 25.50 + 5 × 100 = 678.50`.

## Notas personales

_Qué me costó, errores que cometí, dudas._
