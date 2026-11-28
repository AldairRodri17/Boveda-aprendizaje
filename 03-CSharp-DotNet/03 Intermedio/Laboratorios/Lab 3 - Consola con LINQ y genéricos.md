---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: C# y .NET
herramienta: .NET 10 SDK y VS Code (o LINQPad)
tags: [csharp-dotnet, laboratorio]
contenido: completo
---
# Lab 3 - Consola con LINQ y genéricos

Parte de: [[03-CSharp-DotNet/03 Intermedio/_Índice Intermedio|Intermedio]]

**Cubre:** [[Genéricos]] · [[Constraints de genéricos]] · [[LINQ]] · [[LINQ - Where, Select y SelectMany]] · [[LINQ - GroupBy y Join]] · [[LINQ - agregaciones (Count, Sum, Aggregate)]] · [[LINQ - First, Single, Any y All]] · [[Ejecución diferida en LINQ]] · [[yield]] · [[Métodos de extensión]] · [[Expresiones lambda]] · [[Func, Action y Predicate]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Practicar consultas LINQ sobre datos en memoria, entender la ejecución diferida y escribir utilidades genéricas propias.

## Datos

```csharp
record Venta(int Id, string Vendedor, string Zona, DateOnly Fecha, decimal Monto);
record Zona(string Nombre, string Gerente);

List<Venta> ventas = [
    new(1, "Ana",  "Norte", new(2026, 10, 1), 100m),
    new(2, "Luis", "Sur",   new(2026, 10, 3),  50m),
    new(3, "Ana",  "Sur",   new(2026, 10, 7),  70m),
    new(4, "Eva",  "Norte", new(2026, 11, 2),  30m),
    new(5, "Luis", "Sur",   new(2026, 11, 5), 120m),
    new(6, "Eva",  "Centro",new(2026, 11, 6),  80m),
];
List<Zona> zonas = [ new("Norte", "Marta"), new("Sur", "Raúl") ];
```

## Pasos

1. Total vendido por zona, ordenado de mayor a menor.
2. El mejor vendedor de cada mes (`yyyy-MM`).
3. Ventas con su gerente usando **left join** (la zona Centro no tiene gerente: muestra `"(sin gerente)"`).
4. ¿Todas las ventas de noviembre superan 25? ¿Hay alguna mayor a 110? Usa `All` y `Any`.
5. Escribe el método genérico `static T MaxPor<T, TKey>(this IEnumerable<T> xs, Func<T, TKey> clave) where TKey : IComparable<TKey>` sin usar `MaxBy`.
6. Escribe `static IEnumerable<T[]> EnLotes<T>(this IEnumerable<T> xs, int tamaño)` con `yield` (sin usar `Chunk`) y pruébalo con lotes de 4.
7. Demuestra la ejecución diferida: crea una consulta, agrega una venta después y muestra que el resultado cambia; luego repite con `ToList()`.

## Criterios de terminado

- [ ] Paso 1 imprime `Sur 240`, `Norte 130`, `Centro 80`.
- [ ] Paso 2 imprime `2026-10 Ana 170` y `2026-11 Luis 120`.
- [ ] Paso 3 imprime 6 filas y la venta 6 sale con `(sin gerente)`.
- [ ] Paso 4 imprime `True` y `True`.
- [ ] `ventas.MaxPor(v => v.Monto).Id` es `5`.
- [ ] `EnLotes(4)` sobre las 6 ventas produce lotes de 4 y 2.
- [ ] Puedo explicar con mis palabras por qué cambió el resultado en el paso 7.

## Solución

> [!success]- Pasos 1 a 4
> ```csharp
> var porZona = ventas.GroupBy(v => v.Zona)
>     .Select(g => (Zona: g.Key, Total: g.Sum(v => v.Monto)))
>     .OrderByDescending(x => x.Total);
>
> var mejorPorMes = ventas.GroupBy(v => v.Fecha.ToString("yyyy-MM"))
>     .Select(g => g.GroupBy(v => v.Vendedor)
>                   .Select(x => (Mes: g.Key, Vendedor: x.Key, Total: x.Sum(v => v.Monto)))
>                   .MaxBy(x => x.Total));
>
> var conGerente = from v in ventas
>                  join z in zonas on v.Zona equals z.Nombre into gz
>                  from z in gz.DefaultIfEmpty()
>                  select (v.Id, v.Vendedor, Gerente: z?.Gerente ?? "(sin gerente)");
>
> var nov = ventas.Where(v => v.Fecha.Month == 11);
> Console.WriteLine(nov.All(v => v.Monto > 25));   // True
> Console.WriteLine(ventas.Any(v => v.Monto > 110)); // True
> ```

> [!success]- Pasos 5 y 6
> ```csharp
> public static T MaxPor<T, TKey>(this IEnumerable<T> xs, Func<T, TKey> clave) where TKey : IComparable<TKey>
> {
>     using var e = xs.GetEnumerator();
>     if (!e.MoveNext()) throw new InvalidOperationException("Secuencia vacía");
>     var mejor = e.Current; var k = clave(mejor);
>     while (e.MoveNext()) { var kk = clave(e.Current); if (kk.CompareTo(k) > 0) { mejor = e.Current; k = kk; } }
>     return mejor;
> }
>
> public static IEnumerable<T[]> EnLotes<T>(this IEnumerable<T> xs, int tamaño)
> {
>     ArgumentOutOfRangeException.ThrowIfLessThan(tamaño, 1);
>     return Iterar();
>     IEnumerable<T[]> Iterar()
>     {
>         var lote = new List<T>(tamaño);
>         foreach (var x in xs) { lote.Add(x); if (lote.Count == tamaño) { yield return lote.ToArray(); lote.Clear(); } }
>         if (lote.Count > 0) yield return lote.ToArray();
>     }
> }
> ```
> La función local permite que la validación del argumento falle al llamar, no al enumerar.

> [!success]- Paso 7
> `var q = ventas.Where(v => v.Monto > 100);` no se ejecuta hasta enumerarla. Si agregas una venta de 200 y luego haces `q.Count()`, cuenta 2 (la 5 y la nueva). Con `.ToList()` antes de agregar, se queda en 1.

## Notas personales

_Qué me costó, errores que cometí, dudas._
