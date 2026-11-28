---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: C# y .NET
tags: [csharp-dotnet]
contenido: completo
---
# Constraints de genéricos

Parte de: [[03-CSharp-DotNet/03 Intermedio/_Índice Intermedio|Intermedio]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **restricción** (*constraint*) es una cláusula `where T : ...` que limita qué tipos pueden sustituir a un parámetro genérico. A cambio, dentro del código genérico puedes usar lo que la restricción garantiza: llamar `CompareTo`, crear `new T()`, asignar `null` o acceder a miembros de una clase base. Sin restricciones, `T` solo te deja lo que tiene `object`.

### Cómo funciona
El compilador verifica las restricciones en dos lugares: dentro del genérico (te permite usar los miembros garantizados) y en cada uso (rechaza `Max<Stream>` si `Stream` no cumple). Las restricciones disponibles son:

| Restricción | Significa |
|---|---|
| `where T : class` / `class?` | Tipo de referencia (no nulo / nulo permitido). |
| `where T : struct` | Tipo de valor no anulable (excluye `int?`). |
| `where T : notnull` | Cualquier tipo no anulable, referencia o valor. |
| `where T : unmanaged` | Struct sin referencias, apto para memoria cruda y `Span`. |
| `where T : new()` | Tiene constructor público sin parámetros; permite `new T()`. Va al final. |
| `where T : ClaseBase` | Hereda de esa clase; puedes usar sus miembros. |
| `where T : IInterfaz` | Implementa la interfaz (puedes poner varias). |
| `where T : U` | `T` es o deriva de otro parámetro `U`. |
| `where T : allows ref struct` | (C# 13) Permite pasar `ref struct` como `Span<T>`; es una anti-restricción. |

Se combinan con comas en un orden fijo: primero `class`/`struct`/`notnull`/`unmanaged` o clase base, luego interfaces, al final `new()`. Ejemplo: `where T : class, IEntidad, new()`.

`new()` sirve para fábricas genéricas: "dame una instancia vacía de cualquier tipo". Ten en cuenta que internamente usa `Activator.CreateInstance`, así que no es la vía más rápida en código crítico.

### Ejemplo
El mayor de dos valores de **cualquier** tipo comparable, y una suma usando *generic math* (.NET 7+):

```csharp
using System.Numerics;

public static class Mat
{
    // Cualquier tipo que sepa compararse consigo mismo: int, decimal, string, DateOnly...
    public static T Mayor<T>(T a, T b) where T : IComparable<T>
        => a.CompareTo(b) >= 0 ? a : b;

    // Generic math: INumber<T> expone +, -, Zero, One, etc. como miembros estáticos abstractos
    public static T Sumar<T>(IEnumerable<T> valores) where T : INumber<T>
    {
        T total = T.Zero;
        foreach (var v in valores) total += v;
        return total;
    }

    // Fábrica genérica con new()
    public static T CrearVacio<T>() where T : new() => new T();
}

// Uso
var m1 = Mat.Mayor(3, 9);                                       // 9
var m2 = Mat.Mayor("ana", "beto");                              // "beto"
var total = Mat.Sumar(new[] { 10.5m, 20m });                    // 30.5m (decimal)
var enteros = Mat.Sumar([1, 2, 3]);                             // 6 (int)

// Restricción por interfaz propia en tu dominio
public interface IEntidad { int Id { get; } }

public sealed class CacheEntidades<T> where T : class, IEntidad
{
    private readonly Dictionary<int, T> _porId = [];
    public void Guardar(T e) => _porId[e.Id] = e;   // podemos usar e.Id gracias a la restricción
    public T? Buscar(int id) => _porId.GetValueOrDefault(id); // T? válido porque T : class
}
```

Las interfaces de **generic math** (`INumber<T>`, `IAdditionOperators<,,>`, `IComparisonOperators<,,>`) se basan en *static abstract members* en interfaces. Antes de .NET 7 tenías que escribir `Sumar` una vez por tipo numérico o usar `dynamic`; ahora una sola versión sirve para `int`, `long`, `decimal`, `double` y cualquier tipo propio que implemente esas interfaces.

### Cuándo usarlo y cuándo no
- **Úsalo** cuando el código genérico necesita una capacidad concreta (comparar, construir, leer una propiedad común).
- **Prefiere la restricción más débil** que funcione: `IComparable<T>` y no `struct, IComparable<T>` si también te sirven strings.
- **Evita** encadenar restricciones solo para "documentar"; cada una reduce quién puede usar tu API.
- **Alternativa**: recibir un delegado (`Func<T, T, int>` o `IComparer<T>`) en vez de exigir la interfaz al tipo; así funciona incluso con tipos que no controlas.

### Errores comunes
1. **Usar `IComparable` (no genérico)**: obliga a *boxing* con tipos de valor y pierde seguridad de tipos; usa `IComparable<T>`.
2. **Poner `new()` antes de otras restricciones**: no compila; siempre va al final.
3. **Pensar que `where T : struct` acepta `int?`**: no, excluye `Nullable<T>`.
4. **Llamar `==` sobre `T` sin restricción**: no compila (o compara referencias con `class`); usa `EqualityComparer<T>.Default.Equals(a, b)` o `IEquatable<T>`.
5. **Abusar de `new()` en rutas calientes**: es más lento que un constructor directo; pasa un `Func<T>` de fábrica si importa.

### Para profundizar
- [Restricciones de parámetros de tipo (Microsoft Learn)](https://learn.microsoft.com/dotnet/csharp/programming-guide/generics/constraints-on-type-parameters)
- [Generic math en .NET](https://learn.microsoft.com/dotnet/standard/generics/math)
- [Miembros estáticos abstractos en interfaces](https://learn.microsoft.com/dotnet/csharp/whats-new/tutorials/static-virtual-interface-members)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué restricciones existen?
> `where T : class`, `struct`, `notnull`, `unmanaged`, `new()`, `UnaClaseBase`, `IUnaInterfaz`, `U` (otro parámetro) y desde C# 13 `allows ref struct`.

> [!question]- ¿Para qué sirve `where T : new()`?
> Permite hacer `new T()` dentro del método; exige un constructor público sin parámetros.

> [!question]- Escribe una función que regrese el mayor de dos valores de cualquier tipo comparable.
> ```csharp
> T Max<T>(T a, T b) where T : IComparable<T> => a.CompareTo(b) >= 0 ? a : b;
> ```

> [!question]- ¿Qué aportan las *generic math* interfaces como `INumber<T>`?
> Permiten escribir `Sumar<T>(IEnumerable<T> xs) where T : INumber<T>` usando `+` y `T.Zero` para cualquier tipo numérico.

## Práctica

- [[Lab 3 - Consola con LINQ y genéricos]]

