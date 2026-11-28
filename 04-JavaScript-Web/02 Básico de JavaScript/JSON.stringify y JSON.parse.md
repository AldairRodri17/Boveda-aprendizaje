---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: JavaScript y Web
tags: [javascript-web]
contenido: completo
---
# JSON.stringify y JSON.parse

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**JSON** (*JavaScript Object Notation*) es un formato de texto para intercambiar datos, definido en el RFC 8259. Es lo que envía y recibe tu API de ASP.NET Core. En JavaScript, `JSON.stringify(valor)` convierte un valor a texto JSON y `JSON.parse(texto)` convierte texto JSON en valores.

JSON se parece a la sintaxis de objetos de JS, pero es **más estricto**: claves y cadenas **siempre con comillas dobles**, sin comas finales, sin comentarios, y solo admite objetos, arreglos, cadenas, números finitos, `true`, `false` y `null`.

### Cómo funciona `JSON.stringify`
Recorre el valor y serializa lo que JSON sabe representar. Lo que no, se **omite o se cambia**:

| Valor | En un objeto | En un arreglo |
|---|---|---|
| `undefined`, funciones, `Symbol` | La propiedad **se omite** | Se vuelve `null` |
| `NaN`, `Infinity` | `null` | `null` |
| `Date` | Cadena ISO (llama a `toJSON()`) | Cadena ISO |
| `Map`, `Set` | `{}` (pierde el contenido) | `{}` |
| `BigInt` | **Lanza `TypeError`** | Lanza `TypeError` |
| Referencias circulares | **Lanza `TypeError`** | Lanza `TypeError` |

```js
JSON.stringify({ a: undefined, b: () => 1, c: NaN, d: new Date(0), e: [undefined] });
// '{"c":null,"d":"1970-01-01T00:00:00.000Z","e":[null]}'
```

Si un objeto tiene un método `toJSON()`, `stringify` usa lo que devuelva.

**Segundo argumento (*replacer*)**: una función `(clave, valor) => nuevoValor` para transformar u omitir (devolver `undefined` omite), o un **arreglo de claves permitidas** (lista blanca).

**Tercer argumento (*space*)**: sangría para hacerlo legible: un número de espacios (hasta 10) o una cadena como `'\t'`.

```js
JSON.stringify(usuario, ['id', 'nombre']);               // solo esas claves
JSON.stringify(datos, (k, v) => (k === 'password' ? undefined : v), 2); // quita y sangra
```

### Cómo funciona `JSON.parse`
Analiza el texto y construye valores. Si el texto no es JSON válido, lanza **`SyntaxError`**:

```js
JSON.parse("{'a':1}");   // SyntaxError: comillas simples no son JSON
JSON.parse('{"a":1,}');  // SyntaxError: coma final
JSON.parse('');          // SyntaxError: texto vacío
```

Las fechas **no regresan como `Date`**: llegan como cadenas ISO, porque JSON no tiene tipo fecha. Para convertirlas usa el segundo argumento (*reviver*), que se llama por cada par clave-valor de abajo hacia arriba:

```js
const ISO = /^\d{4}-\d{2}-\d{2}T/;
const pedido = JSON.parse(texto, (k, v) => (typeof v === 'string' && ISO.test(v) ? new Date(v) : v));
```

En la práctica es más claro convertir explícitamente los campos que sabes que son fechas.

### Ejemplo
Guardar y leer un borrador de pedido en `localStorage`, que solo guarda cadenas:

```js
function guardarBorrador(pedido) {
  localStorage.setItem('borrador-pedido', JSON.stringify(pedido));
}

function leerBorrador() {
  const texto = localStorage.getItem('borrador-pedido');
  if (!texto) return null;
  try {
    const p = JSON.parse(texto);
    p.creadoEn = new Date(p.creadoEn);       // reconstruir la fecha
    return p;
  } catch {
    localStorage.removeItem('borrador-pedido'); // dato corrupto: descártalo
    return null;
  }
}
```

Con `fetch`, `await r.json()` hace el `JSON.parse` del cuerpo y también lanza `SyntaxError` si la respuesta no es JSON (por ejemplo, una página HTML de error de un proxy). Revisa `r.ok` y el `Content-Type` antes. Más en [[localStorage]].

### Cuándo usarlo y cuándo no
Úsalo para enviar y recibir datos de APIs, guardar en `localStorage` y registrar objetos en logs. **No** lo uses como forma de copiar objetos: pierde fechas, `Map`, `Set`, `undefined` y falla con ciclos; para eso está [[structuredClone y copia profunda]]. Para números enteros mayores a `Number.MAX_SAFE_INTEGER` (IDs de 64 bits de SQL Server `bigint`), envíalos como cadenas desde la API para no perder precisión.

### Errores comunes
- **Esperar `Date` después de `parse`**.
- **Perder propiedades `undefined`** al serializar (desaparecen).
- **No envolver `JSON.parse` en `try/catch`** con datos externos o de `localStorage`.
- **Serializar `Map`/`Set`** y obtener `{}`: conviértelos antes (`Object.fromEntries(mapa)`, `[...set]`).
- **Doble serialización**: `JSON.stringify` de algo que ya era cadena JSON produce comillas escapadas.

### Para profundizar
- [`JSON.stringify()` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)
- [`JSON.parse()` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)
- [RFC 8259: formato JSON](https://www.rfc-editor.org/rfc/rfc8259)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Predice: `JSON.stringify({a: undefined, b: () => 1, c: NaN, d: new Date(0), e: [undefined]})`.
> `{"c":null,"d":"1970-01-01T00:00:00.000Z","e":[null]}`.

> [!question]- Predice: `JSON.parse("{'a':1}")`.
> `SyntaxError`: JSON exige comillas dobles.

> [!question]- ¿Qué imprime este código?
> ```js
> const o = {}; o.self = o;
> JSON.stringify(o);
> ```
>
> > [!success]- Respuesta
> > `TypeError` (estructura circular).

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué valores se pierden o cambian con `JSON.stringify`?
> `undefined`, funciones y símbolos se omiten en objetos (y se vuelven `null` en arreglos); `NaN`/`Infinity` → `null`; `Date` → string ISO; `Map`/`Set` → `{}`. `BigInt` y referencias circulares lanzan `TypeError`.

> [!question]- ¿Qué devuelve `JSON.parse` para una fecha serializada?
> Un **string**, no un `Date`. Conviértelo tú (o usa el segundo argumento `reviver`).

> [!question]- ¿Para qué sirven el 2.º y 3.er argumento de `stringify`?
> El 2.º (`replacer`) filtra o transforma valores; el 3.º indenta: `JSON.stringify(obj, null, 2)`.

