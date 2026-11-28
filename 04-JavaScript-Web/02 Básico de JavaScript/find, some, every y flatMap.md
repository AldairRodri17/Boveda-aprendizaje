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
# find, some, every y flatMap

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Son métodos de arreglo para **buscar** y **verificar** condiciones, más uno para **transformar y aplanar** a la vez. Complementan a `map`, `filter` y `reduce` (ver [[map, filter y reduce]]) y, a diferencia de `forEach`, varios **se detienen en cuanto conocen la respuesta**.

### Cómo funciona
| Método | Pregunta que responde | Devuelve | Se detiene |
|---|---|---|---|
| `find(fn)` | ¿Cuál es el **primero** que cumple? | El elemento o `undefined` | Al encontrarlo |
| `findIndex(fn)` | ¿En qué **posición** está el primero? | El índice o `-1` | Al encontrarlo |
| `findLast(fn)` / `findLastIndex(fn)` | Igual, buscando desde el final | Elemento / índice | Al encontrarlo |
| `some(fn)` | ¿**Alguno** cumple? | `true`/`false` | En el primer `true` |
| `every(fn)` | ¿**Todos** cumplen? | `true`/`false` | En el primer `false` |
| `flatMap(fn)` | Transforma cada uno y aplana **un nivel** | Arreglo nuevo | Recorre todo |

```js
[1, 2, 3].find(x => x > 5);        // undefined
[1, 2, 3].some(x => x > 2);        // true
[1, 2, 3].every(x => x > 0);       // true
[1, 2, 3].findIndex(x => x === 9); // -1
[[1, 2], [3]].flatMap(x => x);     // [1, 2, 3]
```

### `find` vs `filter`
- **`find`** devuelve **un elemento** (el primero) o `undefined`, y deja de recorrer al encontrarlo.
- **`filter`** devuelve **un arreglo** con **todos** los que cumplen (posiblemente vacío) y siempre recorre todo.

Si solo necesitas uno (buscar por id), `find` es más claro y más rápido. Un error común es `filter(...)[0]`: funciona, pero recorre todo y crea un arreglo intermedio.

Cuidado con `find` cuando el elemento buscado puede ser falsy (`0`, `''`): `undefined` significa "no encontrado", pero si tu arreglo puede contener `undefined`, usa `findIndex` para distinguir.

### Arreglos vacíos: verdad vacía
`[].every(x => false)` es **`true`** y `[].some(x => true)` es **`false`**. Es la lógica matemática de la "verdad vacía": "todos los elementos de un conjunto vacío cumplen X" es verdadero porque no hay ningún contraejemplo. En la práctica, si validas "todos los renglones tienen cantidad > 0" con `every` y el pedido no tiene renglones, la validación **pasa**. Revisa por separado `arr.length > 0` cuando un vacío no deba ser válido.

### `flatMap`
Equivale a `arr.map(fn).flat()` (aplana **un solo nivel**), pero en una pasada. Tiene dos usos muy prácticos:

1. **Uno a muchos**: cada elemento produce varios.
2. **Filtrar y transformar a la vez**: devuelve `[]` para descartar y `[valor]` para conservar.

```js
const pedidos = [
  { id: 1, renglones: [{ sku: 'A1' }, { sku: 'B2' }] },
  { id: 2, renglones: [{ sku: 'A1' }] },
];
pedidos.flatMap(p => p.renglones.map(r => r.sku)); // ['A1', 'B2', 'A1']

// Filtrar + transformar en una sola pasada
['10', 'x', '7'].flatMap(s => {
  const n = Number(s);
  return Number.isFinite(n) ? [n] : [];
});                                                 // [10, 7]
```

Para aplanar más niveles, usa `flat(profundidad)` o `flat(Infinity)`.

### Ejemplo
Validación y búsqueda en la captura de pedidos de Itzamna.Clio:

```js
const pedido = { renglones: [ { sku: 'A1', cant: 2 }, { sku: 'B2', cant: 0 } ] };

const hayRenglones = pedido.renglones.length > 0;
const todosValidos = hayRenglones && pedido.renglones.every(r => r.cant > 0); // false
const primeroInvalido = pedido.renglones.findIndex(r => r.cant <= 0);        // 1
const tieneB2 = pedido.renglones.some(r => r.sku === 'B2');                    // true
const renglonA1 = pedido.renglones.find(r => r.sku === 'A1');                  // { sku: 'A1', cant: 2 }

if (!todosValidos) marcarError(`renglon-${primeroInvalido}`, 'Cantidad inválida');

// Etiquetas únicas de todos los pedidos
const etiquetas = [...new Set(pedidos.flatMap(p => p.etiquetas ?? []))];
```

### Cuándo usarlo y cuándo no
Usa el método cuyo **nombre describe la pregunta**: `some` en lugar de `filter(...).length > 0`, `find` en lugar de `filter(...)[0]`, `every` en lugar de un bucle con bandera. Para búsquedas repetidas por clave sobre muchos elementos, construye una vez un `Map` por id en lugar de hacer `find` en cada consulta (ver [[Map y Set]]). `includes` es más simple cuando buscas un valor primitivo exacto.

### Errores comunes
- **Olvidar que `every` sobre vacío es `true`**.
- **`filter(...)[0]`** en vez de `find`.
- **Esperar `null` de `find`**: devuelve `undefined`.
- **Comprobar `findIndex` como booleano**: `if (arr.findIndex(...))` falla cuando el índice es 0 y pasa cuando es -1. Compara con `!== -1`.
- **`flatMap` esperando aplanar varios niveles**.

### Para profundizar
- [`Array.prototype.find()` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find)
- [`Array.prototype.every()` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/every)
- [`Array.prototype.flatMap()` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Predice: `[1,2,3].find(x => x > 5)`, `[1,2,3].some(x => x > 2)`, `[1,2,3].findIndex(x => x === 9)`.
> `undefined`, `true`, `-1`.

> [!question]- Predice: `[[1,2],[3]].flatMap(x => x)`.
> `[1, 2, 3]`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre `find` y `filter`?
> `find` devuelve el **primer** elemento que cumple (o `undefined`) y se detiene; `filter` recorre todo y devuelve un arreglo.

> [!question]- ¿Qué devuelve `[].every(x => false)`?
> `true` (verdad vacía: ningún elemento la incumple). `[].some(...)` devuelve `false`.

> [!question]- ¿Para qué sirve `flatMap`?
> Mapear y aplanar un nivel en un paso: `pedidos.flatMap(p => p.items)`. También sirve para filtrar y transformar a la vez devolviendo `[]` o `[valor]`.

