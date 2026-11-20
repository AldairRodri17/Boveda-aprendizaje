---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: JavaScript y Web
tags: [javascript-web]
---
# Spread y rest

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Los tres puntos `...` tienen dos usos opuestos según **dónde** aparecen:

- **Spread** (propagación) **expande** un iterable o un objeto en sus elementos: dentro de un arreglo literal, de un objeto literal o de los argumentos de una llamada.
- **Rest** (resto) **junta** varios elementos en uno solo: en los parámetros de una función o en el lado izquierdo de una desestructuración.

Regla para distinguirlos: si `...` está **donde se producen valores** (lado derecho, argumentos de una llamada), es spread; si está **donde se reciben** (parámetros, patrón de desestructuración), es rest.

### Cómo funciona: spread
```js
// Arreglos (funciona con cualquier iterable)
const a = [1, 2];
const b = [...a, 3];                 // [1, 2, 3]
const copia = [...a];                // copia superficial
[...'hola'];                         // ['h', 'o', 'l', 'a']
[...new Set([1, 1, 2])];             // [1, 2]

// Argumentos
Math.max(...[3, 7, 1]);              // 7

// Objetos (propiedades propias y enumerables)
const base = { a: 1, b: 2 };
const extendido = { ...base, c: 3 }; // { a: 1, b: 2, c: 3 }
({ ...{ a: 1, b: 2 }, ...{ b: 3 } }); // { a: 1, b: 3 }
```

**Claves repetidas**: en `{ ...a, ...b }` las propiedades se copian en orden y **gana la última**. Por eso el orden expresa la intención:
- `{ ...defaults, ...opciones }`: las opciones del usuario sobrescriben los valores por defecto.
- `{ ...pedido, estado: 'surtido' }`: copia y cambia un campo (patrón típico de actualización inmutable en React).
- `{ estado: 'surtido', ...pedido }`: aquí el `estado` original de `pedido` **pisa** al nuevo; casi siempre es un error.

### Copia superficial, no profunda
Spread copia **un nivel**: las propiedades que son objetos o arreglos se copian **por referencia**. Modificar un anidado en la copia modifica el original:

```js
const original = { id: 1, cliente: { nombre: 'Ana' }, renglones: [{ cant: 1 }] };
const copia = { ...original };
copia.id = 2;                    // no afecta al original
copia.cliente.nombre = 'Luis';   // ¡sí afecta! mismo objeto
original.cliente.nombre;         // 'Luis'
```

Para actualizar de forma inmutable un anidado, haz spread en cada nivel que cambias:

```js
const actualizado = { ...original, cliente: { ...original.cliente, nombre: 'Luis' } };
```

Para una copia profunda completa, usa `structuredClone` (ver [[structuredClone y copia profunda]]).

### Cómo funciona: rest
```js
// Parámetros: junta los argumentos sobrantes en un arreglo real
function registrar(nivel, ...mensajes) {
  console[nivel](new Date().toISOString(), ...mensajes); // aquí ... es spread
}
registrar('warn', 'Pedido', 1024, 'sin stock');

// Desestructuración de arreglos
const [primero, ...demas] = [10, 20, 30];   // demas = [20, 30]

// Desestructuración de objetos: quitar propiedades
const { password, ...publico } = usuario;   // publico sin password
```

Reglas: solo puede haber **un** rest y debe ir **al final** (`function f(...a, b)` es error de sintaxis). A diferencia del antiguo `arguments`, el parámetro rest es un **arreglo de verdad** (tiene `map`, `filter`) y funciona en funciones flecha. Ver también [[Destructuring]].

### Ejemplo
Combinar opciones de `fetch` en el cliente HTTP de Itzamna.Clio:

```js
const DEFAULTS = { headers: { Accept: 'application/json' }, credentials: 'same-origin' };

export function pedir(ruta, opciones = {}) {
  return fetch(ruta, {
    ...DEFAULTS,
    ...opciones,
    headers: { ...DEFAULTS.headers, ...opciones.headers }, // fusiona el anidado
  });
}

// Quitar campos internos antes de enviar
const { _sucio, _errores, ...dto } = formularioEstado;
await pedir('/api/pedidos', { method: 'POST', body: JSON.stringify(dto) });
```

Sin la línea de `headers`, si el usuario pasa `headers` perdería el `Accept` por defecto, porque el spread superficial reemplaza el objeto completo.

### Qué se copia exactamente
En objetos, el spread copia solo las propiedades **propias y enumerables**: no copia lo heredado del prototipo ni los métodos de una clase (esos viven en el prototipo), ni campos privados `#`. Por eso `{...instancia}` de una clase da un objeto plano sin métodos. Los *getters* se ejecutan y se guarda el valor que devuelven. En arreglos y otros iterables, el spread usa el protocolo de iteración, así que funciona con `Set`, `Map` (da pares), cadenas y `NodeList`, pero **no** con un objeto plano (`[...{a: 1}]` lanza `TypeError` porque los objetos no son iterables).

### Cuándo usarlo y cuándo no
Úsalo para copias superficiales, fusionar opciones, actualizaciones inmutables, convertir iterables a arreglos y funciones con número variable de argumentos. Evita hacer spread de arreglos enormes dentro de llamadas (`Math.max(...unMillon)`), que puede exceder el límite de argumentos; usa `reduce`. En bucles que acumulan, `{ ...acc, [k]: v }` en cada vuelta crea objetos nuevos cada vez (costo cuadrático): muta el acumulador local en ese caso.

### Errores comunes
- **Creer que `{...obj}` es copia profunda**.
- **Orden equivocado** en `{ ...a, ...b }`.
- **Perder anidados** al fusionar configuraciones.
- **Rest que no es el último parámetro**.
- **Spread de `null`/`undefined`**: en objetos se ignora (`{...null}` es `{}`), pero en arreglos lanza `TypeError`.

### Para profundizar
- [Sintaxis spread (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
- [Parámetros rest (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- ¿Qué imprime este código?
> ```js
> const o1 = { p: { q: 1 } };
> const o2 = { ...o1 };
> o2.p.q = 99;
> console.log(o1.p.q);
> ```
>
> > [!success]- Respuesta
> > `99`: copia superficial.

> [!question]- Predice: `{...{a:1, b:2}, ...{b:3}}` y `[...'hola']`.
> `{a: 1, b: 3}` y `['h','o','l','a']`.

> [!question]- ¿Qué imprime este código?
> ```js
> function f(a, ...r) { return r.length; }
> console.log(f(1, 2, 3), f.length);
> ```
>
> > [!success]- Respuesta
> > `2 1`: `f.length` no cuenta el parámetro rest.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre spread y rest si usan la misma sintaxis `...`?
> Spread **expande** (en una llamada, arreglo u objeto literal). Rest **junta** el resto (en parámetros o desestructuración). Depende de dónde aparece.

> [!question]- ¿`{...obj}` hace copia profunda?
> No, copia superficial: los objetos anidados se comparten. Para copia profunda usa `structuredClone`.

> [!question]- ¿Qué pasa con claves repetidas en `{...a, ...b}`?
> Gana la última: las propiedades de `b` sobrescriben las de `a`.

