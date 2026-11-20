---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: C# y .NET
tags: [csharp-dotnet]
---
# Enums

Parte de: [[03-CSharp-DotNet/01 Básico/_Índice Básico|Básico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **enum** (enumeración) es un tipo de valor que define un conjunto de **constantes con nombre**: `EstadoPedido.Pendiente`, `EstadoPedido.Pagado`. Sirve para reemplazar números o strings "mágicos" por nombres que el compilador verifica, que el IDE autocompleta y que se leen solos en el código. Por debajo, cada miembro es un número entero; el tipo subyacente por defecto es **`int`**.

### Cómo funciona
```csharp
public enum EstadoPedido      // : int implícito
{
    Pendiente,                // 0
    Pagado,                   // 1
    Enviado = 10,             // 10
    Cancelado                 // 11 (sigue contando desde el anterior)
}
```
- Puedes cambiar el tipo subyacente: `enum Prioridad : byte { ... }`.
- Convertir entre enum y número requiere casting explícito: `(int)EstadoPedido.Pagado` o `(EstadoPedido)1`.
- **No hay validación automática**: `(EstadoPedido)99` compila y se ejecuta sin error; obtienes un valor que no corresponde a ningún nombre (`ToString()` da `"99"`). Por eso debes validar los valores que vienen de fuera con `Enum.IsDefined(valor)` o con `Enum.TryParse` + `IsDefined`.
- El valor por defecto de un enum es `0`, aunque no haya un miembro con 0. Conviene que el 0 sea un valor válido y "neutro" (`Desconocido` o `Pendiente`).

**`[Flags]`** marca un enum cuyos valores son potencias de 2 para **combinarse** con operadores de bits:
```csharp
[Flags]
public enum Permisos
{
    Ninguno = 0,
    Leer = 1,
    Escribir = 2,
    Borrar = 4,
    Todo = Leer | Escribir | Borrar   // 7
}

var p = Permisos.Leer | Permisos.Escribir;      // 3
bool puedeEscribir = p.HasFlag(Permisos.Escribir);   // true
bool puedeBorrar = (p & Permisos.Borrar) != 0;       // false
Console.WriteLine(p);                                // "Leer, Escribir"
```
El atributo cambia `ToString()` y `Parse` para que manejen combinaciones.

### Ejemplo
Un enum en un slice de API, con validación y serialización como texto:
```csharp
public enum TipoPago { Efectivo = 1, Tarjeta = 2, Transferencia = 3 }

public sealed record RegistrarPago(Guid PedidoId, decimal Monto, TipoPago Tipo);

// FluentValidation: rechazar números que no existen en el enum
public sealed class RegistrarPagoValidator : AbstractValidator<RegistrarPago>
{
    public RegistrarPagoValidator()
    {
        RuleFor(x => x.Tipo).IsInEnum();
        RuleFor(x => x.Monto).GreaterThan(0);
    }
}

// Program.cs: enums como strings en el JSON de entrada y salida
builder.Services.AddControllers()
    .AddJsonOptions(o => o.JsonSerializerOptions.Converters.Add(new JsonStringEnumConverter()));
```
Con el convertidor, la API responde `"tipo": "Tarjeta"` en lugar de `"tipo": 2`, y acepta `"Tarjeta"` al recibir. Para el front en TypeScript (Itzamna.Clio) puedes declarar la unión equivalente: `type TipoPago = "Efectivo" | "Tarjeta" | "Transferencia";`.

Para guardarlo en SQL Server con Dapper, lo habitual es una columna `tinyint`/`int` y pasar el enum como parámetro (Dapper lo envía como su valor numérico). Documenta en el `.sql` qué significa cada número o usa una tabla catálogo.

Recorrer los valores (por ejemplo para llenar un combo):
```csharp
foreach (var tipo in Enum.GetValues<TipoPago>())
    Console.WriteLine($"{(int)tipo} = {tipo}");
```

### Cuándo usarlo y cuándo no
- **Úsalo** para conjuntos pequeños y estables de opciones conocidas al compilar: estados, tipos, modos.
- **Combínalo** con [[switch expressions]] para decidir por estado; el compilador avisa si faltan casos no cubiertos.
- **No** lo uses si las opciones las administra el usuario (catálogos que cambian en la BD) o si cada opción necesita comportamiento propio complejo; en ese caso piensa en una tabla catálogo o en [[Polimorfismo]] (una clase por tipo).
- Si el valor viaja a otros sistemas, decide desde el inicio si se serializa como número o texto y no lo cambies después.

### Errores comunes
- **Confiar en que un enum recibido es válido**: `(TipoPago)99` pasa sin error. Valida con `IsInEnum()` o `Enum.IsDefined`.
- **Reordenar o renumerar miembros** sin valores explícitos: los datos guardados en la BD cambian de significado. Asigna valores explícitos a enums que se persisten.
- **Usar `[Flags]` con valores que no son potencias de 2**: las combinaciones se traslapan.
- **Exponer números en la API**: el front no sabe qué significa `2`. Usa `JsonStringEnumConverter`.
- **`Enum.Parse` con texto del usuario**: lanza excepción si no coincide; usa `Enum.TryParse(texto, ignoreCase: true, out var v)` y además verifica `IsDefined`, porque `TryParse` acepta cadenas numéricas como `"99"`.

### Para profundizar
- [Tipos de enumeración (referencia de C#)](https://learn.microsoft.com/dotnet/csharp/language-reference/builtin-types/enum)
- [Clase System.Enum (API)](https://learn.microsoft.com/dotnet/api/system.enum)
- [JsonStringEnumConverter](https://learn.microsoft.com/dotnet/api/system.text.json.serialization.jsonstringenumconverter)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es un enum y cuál es su tipo subyacente por defecto?
> Un conjunto de constantes con nombre; por defecto es `int` y empieza en 0.

> [!question]- ¿Qué pasa con `(Estado)99` si 99 no está definido?
> Compila y funciona: un enum acepta cualquier valor de su tipo subyacente. Valida con `Enum.IsDefined` o con un validador (FluentValidation `IsInEnum()`).

> [!question]- ¿Para qué sirve `[Flags]`?
> Para combinar valores con bits: `Permiso.Leer | Permiso.Escribir`, y preguntar con `HasFlag`. Los valores deben ser potencias de 2.

> [!question]- Tu API regresa enums como números y el front no los entiende. ¿Qué haces?
> Registra `JsonStringEnumConverter` en las opciones JSON para serializarlos como texto. En SQL decide si guardas el número o el nombre y sé consistente en el mapeo manual.

## Práctica

- [[Lab 1 - Consola de inventario con colecciones y TryParse]]

