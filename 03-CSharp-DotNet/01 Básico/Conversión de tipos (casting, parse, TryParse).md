---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: C# y .NET
tags: [csharp-dotnet]
---
# Conversión de tipos (casting, parse, TryParse)

Parte de: [[03-CSharp-DotNet/01 Básico/_Índice Básico|Básico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Convertir tipos es pasar un valor de un tipo a otro: de `double` a `int`, de `object` a `Cliente`, de un `string` que llegó por HTTP a un número. C# ofrece varias herramientas y cada una tiene reglas distintas de redondeo, de fallo y de cultura:
- **Conversión implícita**: automática cuando no se pierde información (`int` → `long`, `int` → `double`).
- **Casting explícito** `(T)x`: cuando puede perderse información o el tipo real solo se conoce en ejecución.
- **`as` e `is`**: conversiones seguras de tipos de referencia.
- **`Parse` / `TryParse`**: de texto a número, fecha, `Guid`, enum.
- **`Convert.ToX`**: conversiones generales con redondeo y manejo de `null`.

### Cómo funciona
**Casting numérico**: `(int)3.9` **trunca** hacia cero: da `3`; `(int)-3.7` da `-3`. No redondea. Si el valor no cabe (por ejemplo `(int)1e20`), en contexto `unchecked` el resultado no está definido de forma útil; con `checked(...)` lanza `OverflowException`.

**`Convert.ToInt32(double)`** sí redondea, pero al **par más cercano** (redondeo bancario): `Convert.ToInt32(2.5)` es `2` y `Convert.ToInt32(3.5)` es `4`. Si necesitas el redondeo "escolar", usa `Math.Round(x, MidpointRounding.AwayFromZero)`.

**Unboxing**: un `int` guardado en `object` solo se puede sacar como `int` exacto. `object o = 42; long l = (long)o;` compila pero lanza `InvalidCastException`. Lo correcto es `(long)(int)o` o `Convert.ToInt64(o)`.

**`(T)x` vs `as` vs `is`**
| Forma | Si no es del tipo | Funciona con |
|---|---|---|
| `(T)x` | lanza `InvalidCastException` | todo (incluye conversiones definidas por el usuario) |
| `x as T` | regresa `null` | tipos de referencia y nullables |
| `x is T t` | regresa `false`; si es `true` deja `t` asignado | todo |

En código moderno `is T t` es la opción preferida: comprueba y convierte en un paso.

**`Parse` vs `TryParse`**
- `int.Parse(s)` lanza `FormatException` si el texto no es válido (o `OverflowException` si no cabe). Úsalo cuando un fallo es un bug (un valor de configuración que tú controlas).
- `int.TryParse(s, out var n)` regresa `bool` y nunca lanza por formato. Úsalo con entradas del usuario, archivos, query strings.

**Cultura**: `Parse` usa la cultura actual del hilo por defecto. `int.Parse("1,000")` lanza `FormatException` porque el estilo por defecto (`NumberStyles.Integer`) no acepta separador de miles. Peor aún, `double.Parse("1.5")` en una máquina con cultura que usa coma decimal puede fallar o dar otro valor. Para archivos, APIs y bases de datos usa siempre `CultureInfo.InvariantCulture`; para mostrar al usuario, su cultura (`es-MX`).

### Ejemplo
```csharp
using System.Globalization;

// Lectura segura de ?pagina= con 1 por defecto
static int LeerPagina(string? valor) =>
    int.TryParse(valor, NumberStyles.Integer, CultureInfo.InvariantCulture, out var p) && p > 0
        ? p
        : 1;

Console.WriteLine(LeerPagina("3"));    // 3
Console.WriteLine(LeerPagina("abc"));  // 1
Console.WriteLine(LeerPagina(null));   // 1

// Importar montos de un CSV generado por otro sistema
var monto = decimal.Parse("1,234.50",
    NumberStyles.Number, CultureInfo.InvariantCulture);    // 1234.50

// Mostrar al usuario en México
Console.WriteLine(monto.ToString("C", CultureInfo.GetCultureInfo("es-MX"))); // $1,234.50

// Casting de referencias con pattern matching
object recibido = new ArgumentException("x");
if (recibido is ArgumentException ae)
    Console.WriteLine(ae.Message);

// Truncar vs redondear
Console.WriteLine((int)3.9);                                   // 3
Console.WriteLine(Convert.ToInt32(2.5));                       // 2
Console.WriteLine(Math.Round(2.5, MidpointRounding.AwayFromZero)); // 3
```

En una API con controllers, el *model binding* ya hace el `TryParse` por ti (`[FromQuery] int pagina = 1`), y si falla agrega el error al `ModelState`; ver [[Model binding y validación]].

### Cuándo usarlo y cuándo no
- **Implícita**: siempre que el compilador la permita.
- **`(T)x`**: cuando estás seguro del tipo y un fallo sería un bug.
- **`is T t`**: cuando el tipo puede variar.
- **`TryParse`**: toda entrada externa. **`Parse`**: valores internos que deben ser válidos.
- **`Convert`**: cuando el origen es `object` o puede ser `null` (`Convert.ToInt32(null)` da `0`), sabiendo que redondea al par.

### Errores comunes
- **Esperar redondeo de `(int)`**: trunca.
- **Olvidar el redondeo bancario de `Convert`/`Math.Round`**: diferencias de centavos en reportes.
- **Parsear sin cultura**: el mismo código funciona en tu PC y falla en el servidor configurado en otro idioma.
- **Unboxing al tipo equivocado**: `InvalidCastException` aunque los números "quepan".
- **`as` seguido de uso sin comprobar `null`**: cambia una `InvalidCastException` por una `NullReferenceException` más difícil de diagnosticar.

### Para profundizar
- [Conversiones de tipos y casting](https://learn.microsoft.com/dotnet/csharp/programming-guide/types/casting-and-type-conversions)
- [Análisis de cadenas numéricas](https://learn.microsoft.com/dotnet/standard/base-types/parsing-numeric)
- [Operadores is, as y cast](https://learn.microsoft.com/dotnet/csharp/language-reference/operators/type-testing-and-cast)
- [Math.Round y MidpointRounding](https://learn.microsoft.com/dotnet/api/system.math.round)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- ¿Cuánto da `(int)3.9`? ¿Y `(int)-3.7`?
> `3` y `-3`: el cast trunca hacia cero.

> [!question]- ¿Cuánto dan `Convert.ToInt32(2.5)` y `Convert.ToInt32(3.5)`?
> `2` y `4`: `Convert` redondea al par más cercano (redondeo bancario).

> [!question]- ¿Qué pasa con `int.Parse("1,000")`?
> Lanza `FormatException`: el estilo por defecto no acepta separador de miles.

> [!question]- `object o = 42; long l = (long)o;` ¿Qué pasa?
> `InvalidCastException`: un `int` en caja solo se puede *unboxear* a `int`. Correcto: `(long)(int)o`.

> [!question]- Escribe la lectura segura de `?pagina=` a entero con 1 por defecto.
> ```csharp
> int pagina = int.TryParse(texto, out var p) && p > 0 ? p : 1;
> ```

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre `(T)x`, `x as T` y `x is T t`?
> `(T)x` lanza `InvalidCastException` si no se puede. `as` regresa `null` si no se puede (solo tipos de referencia o anulables). `is T t` prueba y asigna en una sola expresión; es lo más idiomático.

> [!question]- ¿Cuándo `int.Parse` y cuándo `int.TryParse`?
> `Parse` cuando un valor inválido es un bug (lanza `FormatException`). `TryParse` cuando el texto viene de fuera (usuario, archivo, query string).

> [!question]- ¿Por qué pasar `CultureInfo.InvariantCulture` al parsear o formatear números y fechas de archivos o APIs?
> Porque la cultura del servidor cambia el separador decimal y el formato de fecha; el mismo texto se interpreta distinto en cada máquina.

## Práctica

- [[Lab 1 - Consola de inventario con colecciones y TryParse]]

