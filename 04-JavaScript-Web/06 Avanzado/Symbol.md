---
proyecto: Conocimiento
tipo: concepto
nivel: avanzado
progreso: pendiente
estado: pendiente
area: JavaScript y Web
tags: [javascript-web]
contenido: completo
---
# Symbol

Parte de: [[04-JavaScript-Web/06 Avanzado/_Índice Avanzado|Avanzado]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`Symbol` es un **tipo primitivo** de JavaScript (junto con string, number, bigint, boolean, undefined y null) cuyos valores son **identificadores únicos**. Cada llamada a `Symbol('descripción')` crea un valor distinto a cualquier otro, aunque la descripción sea la misma. Se usan principalmente como **llaves de propiedades** que no chocan con otras y como "ganchos" para personalizar el comportamiento del lenguaje mediante los *well-known symbols*.

### Cómo funciona
- `const id = Symbol('id')` crea un símbolo. La descripción solo sirve para depurar (`id.description === 'id'`).
- `Symbol('x') === Symbol('x')` es `false`: cada uno es único.
- No se usa `new Symbol()` (lanza `TypeError`): es un primitivo, no un objeto.
- Como llave: `obj[id] = 123`. Las propiedades con llave símbolo:
  - **no aparecen** en `for...in`, `Object.keys`, `Object.entries` ni en `JSON.stringify`;
  - sí aparecen con `Object.getOwnPropertySymbols(obj)` y `Reflect.ownKeys(obj)`.
  Por eso son "semiocultas", no privadas: cualquiera que tenga el símbolo o use esas funciones puede leerlas. Para privacidad real están los campos `#privados` ([[Campos privados en clases JS]]).
- No se convierten implícitamente a string (`'a' + Symbol()` lanza `TypeError`); usa `String(s)` o `s.description`.

#### `Symbol()` vs `Symbol.for()`
- `Symbol('x')`: crea **siempre uno nuevo**, local a donde lo creaste.
- `Symbol.for('x')`: busca en un **registro global** de símbolos (compartido por todo el entorno, incluso entre iframes del mismo agente o módulos distintos); si existe uno con esa llave lo devuelve, si no lo crea. Por eso `Symbol.for('x') === Symbol.for('x')` es `true`.
- `Symbol.keyFor(s)` devuelve la llave si `s` está en el registro global, o `undefined`.
Usa `Symbol.for` cuando dos piezas de código que no se conocen deben acordar el mismo símbolo (por ejemplo, dos copias de una librería en la misma página); usa `Symbol()` cuando quieres unicidad garantizada.

#### *Well-known symbols*
Son símbolos predefinidos como propiedades estáticas de `Symbol` que el propio lenguaje consulta para decidir cómo tratar a tus objetos:
- `Symbol.iterator`: hace a un objeto iterable con `for...of` y spread ([[Generadores e iteradores]]).
- `Symbol.asyncIterator`: para `for await...of`.
- `Symbol.toPrimitive`: controla la conversión a número o string ([[Coerción de tipos]]).
- `Symbol.toStringTag`: personaliza `Object.prototype.toString` (`[object Dinero]`).
- `Symbol.hasInstance`: personaliza `instanceof`.
- Otros: `Symbol.species`, `Symbol.isConcatSpreadable`, `Symbol.match`, `Symbol.replace`.

```mermaid
flowchart LR
    F["for...of obj"] --> I{"¿obj[Symbol.iterator]?"}
    I -- sí --> IT[llama al iterador]
    I -- no --> E[TypeError: no es iterable]
    S["`${obj}` o +obj"] --> P{"¿obj[Symbol.toPrimitive]?"}
    P -- sí --> TP[usa tu conversión]
    P -- no --> VO[valueOf / toString]
```

### Ejemplo
Un tipo `Dinero` que se integra con el lenguaje:

```js
class Dinero {
  #centavos;
  constructor(centavos, moneda = 'MXN') { this.#centavos = centavos; this.moneda = moneda; }

  [Symbol.toPrimitive](pista) {
    if (pista === 'number') return this.#centavos / 100;
    return new Intl.NumberFormat('es-MX', { style: 'currency', currency: this.moneda }).format(this.#centavos / 100);
  }
  get [Symbol.toStringTag]() { return 'Dinero'; }
}

const total = new Dinero(125050);
console.log(`Total: ${total}`);            // "Total: $1,250.50"
console.log(+total);                       // 1250.5
console.log(Object.prototype.toString.call(total)); // "[object Dinero]"
```

Metadatos que no ensucian el objeto ni su JSON:

```js
const ORIGEN = Symbol('origen');

function marcarOrigen(registro, origen) {
  registro[ORIGEN] = origen;              // no choca con ninguna propiedad del API
  return registro;
}

const pedido = marcarOrigen({ id: 7, total: 980 }, 'cache');
JSON.stringify(pedido);   // '{"id":7,"total":980}'  (el símbolo no se serializa)
Object.keys(pedido);      // ['id', 'total']
pedido[ORIGEN];           // 'cache'
```

Registro global:

```js
Symbol.for('app.version') === Symbol.for('app.version'); // true
Symbol('app.version') === Symbol('app.version');         // false
Symbol.keyFor(Symbol.for('app.version'));                // 'app.version'
```

### Cuándo usarlo y cuándo no
- **Sí**: llaves para metadatos en objetos ajenos, constantes únicas que no deben confundirse con strings (tipos de acción internos), implementar protocolos del lenguaje (`Symbol.iterator`, `Symbol.toPrimitive`) y en librerías para evitar colisiones.
- **No** para privacidad: usa `#campos`. Tampoco para datos que deban viajar en JSON o guardarse en `localStorage`: se pierden.
- **No** para valores que deban compararse entre ventanas o procesos distintos, salvo con `Symbol.for` y sabiendo que el registro es por entorno.

### Errores comunes
- **Esperar que `Symbol('a') === Symbol('a')`**.
- **Concatenar un símbolo con texto** (`TypeError`); usa `.description`.
- **Creer que son privados**: `Reflect.ownKeys` los muestra.
- **Perderlos al serializar** con `JSON.stringify` o copiar con spread (el spread **sí** copia propiedades símbolo enumerables; `JSON` no).
- **Usar `Symbol.for` con nombres genéricos** (`'id'`): cualquiera puede obtener el mismo símbolo; usa prefijos (`'miapp.id'`).

### Para profundizar
- [MDN: Symbol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)
- [MDN: Symbol.for()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol/for)
- [MDN: Symbol.toPrimitive](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol/toPrimitive)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es un `Symbol`?
> Un primitivo único: `Symbol('id') !== Symbol('id')`. Se usa como clave de propiedad que no choca con otras y no aparece en `Object.keys` ni en `JSON.stringify`.

> [!question]- ¿Qué son los *well-known symbols*?
> Símbolos predefinidos que personalizan el comportamiento del lenguaje: `Symbol.iterator` (hacer iterable un objeto), `Symbol.asyncIterator`, `Symbol.toPrimitive`, `Symbol.hasInstance`.

> [!question]- ¿Qué diferencia hay entre `Symbol('x')` y `Symbol.for('x')`?
> `Symbol.for` usa un registro global: devuelve el mismo símbolo para la misma clave en todo el programa. `Symbol('x')` siempre crea uno nuevo.

