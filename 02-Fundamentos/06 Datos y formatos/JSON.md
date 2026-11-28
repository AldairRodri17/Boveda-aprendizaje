---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Fundamentos
tags: [fundamentos]
contenido: completo
---
# JSON

Parte de: [[02-Fundamentos/06 Datos y formatos/_Índice Datos y formatos|Datos y formatos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**JSON** (JavaScript Object Notation, RFC 8259) es un formato de texto para representar datos estructurados con una sintaxis mínima tomada de JavaScript. Es el formato por defecto de las APIs web, de archivos de configuración (`appsettings.json`, `package.json`) y de muchos sistemas de mensajería. Su ventaja es que lo lee una persona y lo parsea cualquier lenguaje.

### Cómo funciona
JSON tiene **seis tipos** de valores:

| Tipo | Ejemplo | Nota |
|---|---|---|
| Objeto | `{"id": "P-1", "activo": true}` | Claves **siempre** cadenas con comillas dobles |
| Arreglo | `[1, 2, 3]` | Ordenado, puede mezclar tipos |
| Cadena | `"Café"` | Comillas **dobles**; escapes `\"`, `\\`, `\n`, `é` |
| Número | `1250.5`, `-3`, `1e6` | Sin `NaN`, `Infinity`, ceros a la izquierda ni `+` inicial |
| Booleano | `true`, `false` | En minúsculas |
| Nulo | `null` | |

Lo que **no** tiene, y que causa la mayoría de los errores:
- **Fechas**: se mandan como cadena, idealmente en ISO 8601 (ver [[ISO 8601]]).
- **Decimales exactos**: el estándar no fija precisión; muchos parsers usan doble precisión (ver más abajo).
- **Comentarios** y **comas finales**: no son válidos (aunque algunos lectores, como el de configuración de .NET, los toleran).
- **Comillas simples** y **claves sin comillas**: no son válidas.

Por eso `{'nombre': 'Ana', edad: 30,}` es inválido por tres razones (comillas simples, clave sin comillas y coma final); lo válido es `{"nombre": "Ana", "edad": 30}`.

**Números grandes y JavaScript.** En JS todos los números son doble precisión IEEE 754, que solo representa **enteros exactos hasta 2^53 − 1** (`Number.MAX_SAFE_INTEGER = 9007199254740991`). Un id `9007199254740993` llega a JS como `9007199254740992`: se redondea en silencio al parsear. Por eso los ids de 64 bits (`bigint` de SQL Server, ids tipo Snowflake) y los montos de dinero se envían como **cadena**.

**Serialización en JS.** `JSON.stringify` omite propiedades con `undefined` y funciones, convierte `Date` con `toISOString()` y respeta `null`:

```js
JSON.stringify({ a: undefined, b: null, c: () => 1, d: new Date(0) });
// '{"b":null,"d":"1970-01-01T00:00:00.000Z"}'
```

Al revés, `JSON.parse` **no** convierte cadenas a `Date`: tienes que hacerlo tú.

**System.Text.Json en ASP.NET Core.** Los controllers usan las opciones **web** (`JsonSerializerDefaults.Web`): propiedades en **camelCase** (`NombreCompleto` → `nombreCompleto`), deserialización **sin distinguir mayúsculas** y lectura de números desde cadenas permitida. Fuera de ASP.NET (`JsonSerializer.Serialize(obj)` sin opciones) los nombres quedan tal cual (PascalCase) y la deserialización **sí** distingue mayúsculas. Esa diferencia explica muchos "llega todo en null" en consolas o pruebas.

### Ejemplo
Un pedido con el total como cadena para no perder precisión:

```json
{
  "id": "P-1",
  "total": "1250.50",
  "moneda": "MXN",
  "fechaUtc": "2026-11-09T18:30:00Z",
  "partidas": [
    { "sku": "A1", "cantidad": 2 },
    { "sku": "B7", "cantidad": 1 }
  ]
}
```

En Itzamna.Dionisio, el DTO y la configuración:

```csharp
public sealed record PartidaDto(string Sku, int Cantidad);

public sealed record PedidoDto(
    string Id,
    [property: JsonNumberHandling(JsonNumberHandling.WriteAsString | JsonNumberHandling.AllowReadingFromString)]
    decimal Total,                       // sale como "1250.50"
    string Moneda,
    DateTimeOffset FechaUtc,             // sale en ISO 8601
    IReadOnlyList<PartidaDto> Partidas);

// Program.cs: ajustes globales (camelCase ya viene por defecto en controllers)
builder.Services.AddControllers().AddJsonOptions(o =>
{
    o.JsonSerializerOptions.DefaultIgnoreCondition = JsonIgnoreCondition.WhenWritingNull;
    o.JsonSerializerOptions.Converters.Add(new JsonStringEnumConverter()); // enums como texto
});
```

Y en Itzamna.Clio (TypeScript), el tipo refleja que el total llega como cadena:

```ts
type Pedido = { id: string; total: string; moneda: "MXN" | "USD"; fechaUtc: string; partidas: { sku: string; cantidad: number }[] };
const p: Pedido = await (await fetch("/api/pedidos/P-1")).json();
const fecha = new Date(p.fechaUtc); // conversión explícita
```

### Cuándo usarlo y cuándo no
- **Sí**: APIs REST, configuración, mensajes entre servicios, logs estructurados.
- **XML**: cuando el estándar o el gobierno lo exige (CFDI del SAT, SOAP), o necesitas namespaces y validación por esquema (ver [[XML]]).
- **CSV**: datos tabulares para Excel o cargas masivas (ver [[CSV]]).
- **Formatos binarios** (Protobuf, MessagePack): alto volumen o baja latencia, como en gRPC.

### Errores comunes
1. **Ids o montos grandes como número.** Se corrompen en JS. Envíalos como cadena.
2. **Fechas en formatos locales** (`"09/11/2026"`). Usa ISO 8601 con zona.
3. **Diferencias de mayúsculas** entre serializadores con y sin opciones web. Define las opciones explícitamente en consolas y pruebas.
4. **Parsear JSON con regex o concatenar cadenas** para generarlo. Usa siempre el serializador; evitas JSON inválido e inyecciones.
5. **Confiar en el orden de las propiedades.** El estándar no lo garantiza para objetos; si importa, usa arreglos.

### Para profundizar
- [RFC 8259: The JSON Data Interchange Format](https://www.rfc-editor.org/rfc/rfc8259)
- [Microsoft Learn: serialización JSON con System.Text.Json](https://learn.microsoft.com/dotnet/standard/serialization/system-text-json/overview)
- [MDN: JSON.stringify()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- ¿Es válido este JSON? `{'nombre': 'Ana', edad: 30,}`
> No: claves y cadenas deben ir en comillas dobles, `edad` sin comillas y la coma final no se permite. Válido: `{"nombre": "Ana", "edad": 30}`.

> [!question]- Representa en JSON un pedido con id `"P-1"`, total `1250.50` (como cadena para no perder precisión) y dos partidas con `sku` y `cantidad`.
> ```json
> {"id": "P-1", "total": "1250.50", "partidas": [{"sku": "A1", "cantidad": 2}, {"sku": "B7", "cantidad": 1}]}
> ```

> [!question]- ¿Qué imprime `JSON.stringify({a: undefined, b: null, c: () => 1, d: new Date(0)})` en JS?
> `{"b":null,"d":"1970-01-01T00:00:00.000Z"}`: `undefined` y funciones se omiten y la fecha se convierte con `toISOString()`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué tipos de datos tiene JSON?
> Objeto, arreglo, cadena (con comillas dobles), número, `true`/`false` y `null`. No tiene fechas, decimales exactos, comentarios ni comas finales.

> [!question]- ¿Por qué un id `9007199254740993` llega cambiado a JavaScript?
> JS usa doble precisión: los enteros solo son exactos hasta `2^53 − 1` (`9007199254740991`). Los ids grandes se envían como cadena.

> [!question]- ¿Qué convención de nombres usa por defecto `System.Text.Json` en ASP.NET Core?
> camelCase para propiedades (`NombreCompleto` → `nombreCompleto`) y no distingue mayúsculas al deserializar en web defaults. Fuera de ASP.NET, `JsonSerializer` por defecto respeta el nombre tal cual y distingue mayúsculas.

