---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Java
herramienta: JDK 21, jshell o VS Code; .NET SDK para la comparación
tags: [java, laboratorio]
contenido: completo
---
# Lab 3 - Colecciones y streams de Java comparados con LINQ

Parte de: [[15-Java/_Índice Java|Java]]

**Cubre:** [[Java - colecciones y streams]] · [[Colecciones en Java (List, Set y Map)]] · [[Streams en Java (map, filter y collect)]] · [[Java vs CSharp (comparativa)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Resolver las mismas 5 consultas sobre una lista de ventas con streams de Java y con LINQ en C#, y comparar resultados y estilo.

## Pasos
1. Define en Java `record Venta(String vendedor, String region, BigDecimal monto, LocalDate fecha)` y una lista con al menos 10 ventas en 3 regiones.
2. Define lo mismo en C# con un `record` y una lista igual.
3. Resuelve en ambos lenguajes:
   1. Total vendido.
   2. Ventas mayores a 1000, ordenadas por monto descendente.
   3. Total por región (`Map`/`Dictionary`).
   4. Vendedor con más ventas (cantidad).
   5. Nombres únicos de vendedores ordenados alfabéticamente.
4. Compara los resultados: deben coincidir.
5. Intenta recorrer dos veces el mismo `Stream` en Java y anota qué pasa; haz lo mismo con un `IEnumerable` en C#.

## Criterios de terminado
- [ ] Las 5 consultas dan el mismo resultado en Java y en C#.
- [ ] Usaste `Collectors.groupingBy` y su equivalente `GroupBy`.
- [ ] Viste la `IllegalStateException` al reutilizar un stream.
- [ ] Anotaste al menos 3 diferencias de estilo (por ejemplo `BigDecimal` vs `decimal`).

## Solución

> [!success]- Java
> ```java
> var total = ventas.stream().map(Venta::monto).reduce(BigDecimal.ZERO, BigDecimal::add);
>
> var grandes = ventas.stream()
>     .filter(v -> v.monto().compareTo(new BigDecimal("1000")) > 0)
>     .sorted(Comparator.comparing(Venta::monto).reversed())
>     .toList();
>
> Map<String, BigDecimal> porRegion = ventas.stream().collect(
>     Collectors.groupingBy(Venta::region,
>         Collectors.reducing(BigDecimal.ZERO, Venta::monto, BigDecimal::add)));
>
> var top = ventas.stream()
>     .collect(Collectors.groupingBy(Venta::vendedor, Collectors.counting()))
>     .entrySet().stream().max(Map.Entry.comparingByValue()).map(Map.Entry::getKey).orElse(null);
>
> var nombres = ventas.stream().map(Venta::vendedor).distinct().sorted().toList();
> ```

> [!success]- C#
> ```csharp
> var total = ventas.Sum(v => v.Monto);
> var grandes = ventas.Where(v => v.Monto > 1000).OrderByDescending(v => v.Monto).ToList();
> var porRegion = ventas.GroupBy(v => v.Region).ToDictionary(g => g.Key, g => g.Sum(v => v.Monto));
> var top = ventas.GroupBy(v => v.Vendedor).MaxBy(g => g.Count())?.Key;
> var nombres = ventas.Select(v => v.Vendedor).Distinct().Order().ToList();
> ```

> [!success]- Diferencias que deberías notar
> - `BigDecimal` no tiene operadores: se usa `add`, `compareTo`. En C#, `decimal` es un tipo de valor con operadores.
> - Java necesita `Collectors` para agrupar y sumar; LINQ tiene `Sum`, `MaxBy` directo.
> - Reutilizar un `Stream` lanza `IllegalStateException: stream has already been operated upon or closed`; un `IEnumerable` se vuelve a ejecutar sin error.

## Notas personales

_Qué me costó, errores que cometí, dudas._
