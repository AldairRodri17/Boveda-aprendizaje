---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: C# y .NET
herramienta: .NET 10 SDK, VS Code y xUnit
tags: [csharp-dotnet, laboratorio]
contenido: completo
---
# Lab 2 - Modelo de dominio con encapsulamiento y records

Parte de: [[03-CSharp-DotNet/02 Orientación a objetos/_Índice Orientación a objetos|Orientación a objetos]]

**Cubre:** [[Encapsulamiento]] · [[Constructores]] · [[record]] · [[record struct]] · [[init y required]] · [[Interfaces]] · [[Programar contra interfaces]] · [[Igualdad por valor vs por referencia]] · [[Polimorfismo]] · [[sealed y override]] · [[Clases anidadas y DTOs]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Modelar un `Egreso` que no pueda quedar en un estado inválido, con ids fuertemente tipados, comisiones polimórficas y un DTO mapeado a mano.

## Pasos

1. Crea `Dominio` (classlib) y `Dominio.Tests` (xunit) en una solución.
2. Crea `public readonly record struct EgresoId(long Valor);` y `public sealed record Dinero(decimal Monto, string Moneda)` que proteja su invariante en el constructor (lanza `ArgumentOutOfRangeException` si `Monto < 0` o si `Moneda` no tiene 3 letras).
3. Crea la entidad `public sealed class Egreso` con:
   - propiedades con `private set`;
   - una fábrica `static Result<Egreso> Crear(EgresoId id, decimal monto, string moneda)` que valide las reglas **antes** de construir `Dinero` y regrese un `Result` (sin lanzar);
   - una fábrica `static Egreso Rehydrate(...)` que **no** valide (datos que vienen de la BD);
   - métodos `Autorizar()` y `Cancelar()` que solo permitan transiciones válidas (`Pendiente → Autorizado`, `Pendiente → Cancelado`).
4. Define `interface IComision { Dinero Calcular(Dinero monto); }` con dos implementaciones `sealed`: `ComisionFija` y `ComisionPorcentaje`.
5. Crea `EgresoDto` (record) y un método de extensión `ToDto()` escrito a mano.
6. Escribe pruebas para cada regla.

## Criterios de terminado

- [ ] Desde fuera del ensamblado no se puede asignar `Estado` (no compila).
- [ ] `Crear` con monto negativo o cero regresa un `Result` fallido con código `validacion`, sin lanzar.
- [ ] `Cancelar()` sobre un egreso `Autorizado` regresa error `transicion_invalida`.
- [ ] `new Dinero(10, "MXN") == new Dinero(10, "MXN")` es `true` y hay una prueba que lo demuestra.
- [ ] Una prueba muestra que `EgresoId(1)` no se puede pasar donde se espera un `CajaId(1)` (comentario con el error de compilación).
- [ ] Las dos comisiones se usan a través de `IComision` en una `[Theory]`.
- [ ] Todas las pruebas pasan con `dotnet test`.

## Solución

> [!success]- Entidad
> ```csharp
> public sealed class Egreso
> {
>     public EgresoId Id { get; private set; }
>     public Dinero Monto { get; private set; } = null!;
>     public EstadoEgreso Estado { get; private set; }
>
>     private Egreso() { }
>
>     public static Result<Egreso> Crear(EgresoId id, decimal monto, string moneda)
>     {
>         if (monto <= 0) return Result<Egreso>.Falla("validacion", "El monto debe ser mayor a cero");
>         if (moneda is not { Length: 3 }) return Result<Egreso>.Falla("validacion", "Moneda inválida");
>         return Result<Egreso>.Ok(new Egreso { Id = id, Monto = new Dinero(monto, moneda), Estado = EstadoEgreso.Pendiente });
>     }
>
>     public static Egreso Rehydrate(EgresoId id, Dinero monto, EstadoEgreso estado) =>
>         new() { Id = id, Monto = monto, Estado = estado };
>
>     public Result Cancelar() => Estado switch
>     {
>         EstadoEgreso.Pendiente => Cambiar(EstadoEgreso.Cancelado),
>         _ => Result.Falla("transicion_invalida", $"No se puede cancelar un egreso {Estado}")
>     };
>
>     private Result Cambiar(EstadoEgreso nuevo) { Estado = nuevo; return Result.Ok(); }
> }
> ```
> `Crear` valida reglas de negocio y regresa `Result`; el constructor de `Dinero` solo protege su invariante (si alguien lo usa mal, es un bug y lanza).

> [!success]- Comisiones
> ```csharp
> public sealed class ComisionFija(decimal cuota) : IComision
> {
>     public Dinero Calcular(Dinero m) => m with { Monto = cuota };
> }
> public sealed class ComisionPorcentaje(decimal pct) : IComision
> {
>     public Dinero Calcular(Dinero m) => m with { Monto = Math.Round(m.Monto * pct / 100, 2) };
> }
> ```

## Notas personales

_Qué me costó, errores que cometí, dudas._
