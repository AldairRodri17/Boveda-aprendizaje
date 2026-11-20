---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: JavaScript y Web
tags: [javascript-web]
---
# structuredClone y copia profunda

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`structuredClone(valor)` es una función global (disponible en los navegadores modernos y en Node desde la versión 17) que crea una **copia profunda** de un valor: copia el objeto y, recursivamente, todo lo que contiene, de modo que modificar la copia nunca afecta al original. Usa el **algoritmo de clonación estructurada**, el mismo con el que el navegador copia datos al enviarlos a un Web Worker con `postMessage` o al guardarlos en IndexedDB.

### Copia superficial vs profunda
- **Superficial** (`{...obj}`, `[...arr]`, `Object.assign`, `arr.slice()`): copia el primer nivel; los objetos anidados siguen **compartidos**. Ver [[Spread y rest]].
- **Profunda**: duplica todos los niveles; no queda nada compartido.

```js
const original = { cliente: { nombre: 'Ana' }, renglones: [{ cant: 1 }] };

const sup = { ...original };
sup.cliente.nombre = 'Luis';
original.cliente.nombre;      // 'Luis' → compartido

const prof = structuredClone(original);
prof.renglones[0].cant = 99;
original.renglones[0].cant;   // 1 → independiente
```

### Cómo funciona
El algoritmo recorre el valor y reconstruye cada parte. **Sí** clona:
- primitivos (incluido `BigInt`; `undefined`, `NaN` e `Infinity` se conservan),
- objetos planos y arreglos,
- `Date` (sigue siendo `Date`), `RegExp`, `Map`, `Set`,
- `ArrayBuffer`, *typed arrays*, `Blob`, `File`, `ImageData`,
- la mayoría de los errores nativos (`Error`, `TypeError`...),
- **referencias circulares** y referencias repetidas (las mantiene: si dos propiedades apuntaban al mismo objeto, en la copia también apuntan a una misma copia).

**No** puede clonar (lanza `DataCloneError`):
- **funciones** (incluidos métodos dentro del objeto),
- **nodos del DOM**,
- `Symbol` como valores, objetos `WeakMap`/`WeakSet`, `Proxy` de forma fiable.

Y **pierde** información:
- el **prototipo**: una instancia de clase se clona como **objeto plano**; `instanceof` deja de funcionar y los métodos desaparecen,
- **campos privados** `#` (ver [[Campos privados en clases JS]]),
- *getters* y *setters* (se guarda el valor calculado, no el accesor),
- propiedades no enumerables y claves `Symbol`.

```js
class Pedido { #interno = 1; folio = 7; get total() { return 10; } }
const c = structuredClone(new Pedido());
c;                       // { folio: 7 } → objeto plano
c instanceof Pedido;     // false

structuredClone({ alGuardar() {} }); // DataCloneError
```

### Por qué es mejor que `JSON.parse(JSON.stringify(obj))`
El truco de JSON fue durante años la "copia profunda barata", pero tiene muchos problemas (ver [[JSON.stringify y JSON.parse]]):

| | `JSON.parse(JSON.stringify(x))` | `structuredClone(x)` |
|---|---|---|
| `Date` | Se vuelve **cadena** | Sigue siendo `Date` |
| `Map`, `Set` | Se vuelven `{}` | Se conservan |
| `undefined` en objetos | La propiedad **desaparece** | Se conserva |
| `NaN`, `Infinity` | Se vuelven `null` | Se conservan |
| `BigInt` | Lanza `TypeError` | Se conserva |
| Referencias circulares | Lanza `TypeError` | Se conservan |
| Funciones | Se omiten en silencio | Lanza error (falla claro) |

Además, `structuredClone` expresa la intención ("quiero una copia") y no pasa por una cadena intermedia.

### Ejemplo
Formulario de edición en Itzamna.Clio con opción de "Cancelar cambios":

```js
let original = null;
let borrador = null;

async function editar(id) {
  original = await (await fetch(`/api/pedidos/${id}`)).json();
  original.creadoEn = new Date(original.creadoEn);
  borrador = structuredClone(original);       // el usuario edita el borrador
}

function cancelar() {
  borrador = structuredClone(original);       // regresa al estado inicial
}

function haCambiado() {
  return JSON.stringify(borrador) !== JSON.stringify(original); // comparación simple
}
```

Si el objeto incluye una instancia de clase con métodos (por ejemplo, un `ClienteApi`), sepáralo de los datos antes de clonar: clona solo el estado, no los servicios.

### Cuándo usarlo y cuándo no
Úsalo para duplicar **datos** (estado, respuestas de API, configuraciones) antes de editarlos o guardar un "antes" para deshacer. No lo uses:
- en cada actualización de estado de React por costumbre: copiar todo es caro; normalmente basta con copias superficiales del camino que cambia;
- para objetos con comportamiento (clases, funciones): ahí necesitas un método `clonar()` propio;
- en bucles con objetos enormes sin medir el costo.

### Errores comunes
- **Creer que `{...obj}` es profunda**.
- **Clonar instancias de clase** y esperar que sigan teniendo métodos.
- **Clonar objetos con funciones** (por ejemplo, un estado que guarda callbacks): `DataCloneError`.
- **Seguir usando el truco de JSON** con fechas y perderlas.

### Para profundizar
- [`structuredClone()` (MDN)](https://developer.mozilla.org/en-US/docs/Web/API/Window/structuredClone)
- [Algoritmo de clonación estructurada (MDN)](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Structured_clone_algorithm)
- [Especificación HTML: structured serialization (WHATWG)](https://html.spec.whatwg.org/multipage/structured-data.html)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- ¿Qué imprime este código?
> ```js
> const o = { d: new Date(0), m: new Map([[1, 2]]), n: { x: 1 } };
> const c = structuredClone(o);
> c.n.x = 2;
> console.log(o.n.x, c.d instanceof Date, c.m.get(1));
> ```
>
> > [!success]- Respuesta
> > `1 true 2`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace `structuredClone(obj)`?
> Una copia profunda con el algoritmo de clonado estructurado: copia objetos anidados, `Date`, `Map`, `Set`, arreglos tipados y referencias circulares.

> [!question]- ¿Qué no puede clonar?
> Funciones, nodos del DOM y algunos objetos del entorno: lanza `DataCloneError`. Las instancias de clases propias pierden su prototipo (quedan como objetos planos).

> [!question]- ¿Por qué es mejor que `JSON.parse(JSON.stringify(obj))`?
> El truco JSON pierde `undefined`, convierte fechas a string, rompe `Map`/`Set` y falla con referencias circulares.

