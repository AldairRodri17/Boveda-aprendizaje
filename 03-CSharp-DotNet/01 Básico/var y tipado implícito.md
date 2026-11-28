---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: C# y .NET
tags: [csharp-dotnet]
contenido: completo
---
# var y tipado implícito

Parte de: [[03-CSharp-DotNet/01 Básico/_Índice Básico|Básico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`var` permite declarar una variable local **sin escribir su tipo**: el compilador lo **infiere** de la expresión de la derecha. La variable sigue siendo de **tipado estático y fuerte**: su tipo se decide al compilar y no cambia nunca. `var` es solo una forma más corta de escribir, no una variable dinámica.

### Cómo funciona
```csharp
var total = 0m;                         // decimal
var clientes = new List<Cliente>();     // List<Cliente>
var porId = clientes.ToDictionary(c => c.Id);   // Dictionary<Guid, Cliente>
```
1. El compilador evalúa el tipo de la expresión de inicialización.
2. Ese tipo queda fijo. Asignar después otro tipo es error de compilación:
   ```csharp
   var n = 5;
   n = "cinco";   // CS0029: no se puede convertir string a int
   ```
3. El IL generado es idéntico al de escribir el tipo explícito: no hay costo en tiempo de ejecución.

**Reglas**:
- Solo para **variables locales** (incluidas las de `for`, `foreach`, `using`, `out var`). No sirve para campos, propiedades, parámetros ni tipos de retorno.
- **Debe inicializarse** en la misma línea, porque de ahí sale el tipo.
- `var x = null;` **no compila** (CS0815): `null` no tiene tipo; el compilador no sabe si es `string`, `Cliente` o `int?`.
- Es **obligatorio** con tipos anónimos: `var fila = new { Id = 1, Nombre = "Ana" };` no tiene nombre de tipo que puedas escribir.

**`var` vs `dynamic`**: `dynamic` sí desactiva la verificación en compilación y resuelve miembros en ejecución (con riesgo de `RuntimeBinderException`). Son cosas opuestas.

**Alternativa: `new()` con tipo destino** (C# 9). Si prefieres ver el tipo a la izquierda:
```csharp
List<Cliente> clientes = new();
Dictionary<string, decimal> totales = new(StringComparer.OrdinalIgnoreCase);
```

```mermaid
flowchart LR
    E["Expresión derecha"] --> C["Compilador infiere tipo"] --> F["Tipo fijo para siempre<br/>(igual que escribirlo)"]
```

### Ejemplo
Casos donde `var` ayuda (el tipo es obvio o muy largo):
```csharp
var conexion = new SqlConnection(cadena);                  // obvio por el new
var resultado = await sql.EjecutarAsync(query, ct);        // tipo largo y genérico
foreach (var (cuenta, total) in totalesPorCuenta)          // deconstrucción
    Console.WriteLine($"{cuenta}: {total:C}");

var resumen = egresos
    .GroupBy(e => e.Mes)
    .Select(g => new { Mes = g.Key, Total = g.Sum(e => e.Monto) });   // tipo anónimo
```

Casos donde conviene **escribir el tipo**:
```csharp
// ¿Qué regresa? No es obvio leyendo la línea
var x = ObtenerDatos();                 // ¿List? ¿IEnumerable diferido? ¿Task?
IReadOnlyList<Egreso> x2 = ObtenerDatos();   // claro

// El tipo numérico importa
var monto = 10;          // int, aunque quizá querías decimal
decimal monto2 = 10;     // explícito

// Quieres trabajar contra una interfaz, no contra la implementación
IEnumerable<Egreso> fuente = new List<Egreso>();   // con var serías List<Egreso>
```

### Cuándo usarlo y cuándo no
**Úsalo** cuando:
- El tipo es evidente en la misma línea (`new`, un cast, un literal claro).
- El tipo es muy largo y no aporta (`Dictionary<string, List<(Guid, decimal)>>`).
- Es un tipo anónimo o una deconstrucción.

**No lo uses** cuando:
- El tipo **no se deduce** leyendo la línea (resultado de un método con nombre ambiguo); en code review en Azure DevOps no hay IDE que te muestre el tipo al pasar el mouse.
- El **tipo numérico** importa (`int` vs `decimal` vs `double`).
- Quieres que la variable sea de una **interfaz o tipo base** distinto al de la expresión.

Lo importante es que el equipo tenga una regla y la haga cumplir con `.editorconfig` (`csharp_style_var_when_type_is_apparent`, `csharp_style_var_elsewhere`, etc.), en lugar de discutirlo en cada PR.

### Errores comunes
- **Creer que `var` es dinámico o "como JavaScript"**: el tipo es fijo desde que se compila.
- **Inferir el numérico equivocado**: `var tasa = 0.16;` es `double`, no `decimal`; en cálculos de dinero mezclarlo con `decimal` no compila o pierde precisión.
- **`var` con métodos que regresan `IEnumerable` diferido** sin darse cuenta: la consulta se ejecuta cada vez que la recorres (ver [[Ejecución diferida en LINQ]]).
- **Querer usar `var` en campos o parámetros**: no está permitido.

### Para profundizar
- [Variables locales con asignación implícita de tipos](https://learn.microsoft.com/dotnet/csharp/language-reference/statements/declarations#implicitly-typed-local-variables)
- [Convenciones de código de C# (uso de var)](https://learn.microsoft.com/dotnet/csharp/fundamentals/coding-style/coding-conventions)
- [Reglas de estilo IDE0007 e IDE0008 (var)](https://learn.microsoft.com/dotnet/fundamentals/code-analysis/style-rules/ide0007-ide0008)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿`var` hace que la variable sea dinámica?
> No. El tipo se infiere **en compilación** a partir de la expresión de la derecha y no cambia. `var x = 5; x = "a";` no compila. Lo dinámico es `dynamic`.

> [!question]- ¿Por qué `var x = null;` no compila?
> Porque `null` no tiene tipo propio y el compilador no puede inferirlo. Escribe el tipo: `string? x = null;`.

> [!question]- ¿Cuándo conviene **no** usar `var`?
> Cuando el tipo no es evidente y importa al leer, p. ej. `var total = Calcular();` (¿`int`? ¿`decimal`?). Con `new`, LINQ o tipos anónimos `var` es lo normal. Con `new()` (target-typed) puedes escribir `List<int> l = new();`.

