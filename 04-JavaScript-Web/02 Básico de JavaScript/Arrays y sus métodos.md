---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: JavaScript y Web
tags: [javascript-web]
---
# Arrays y sus métodos

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **arreglo** (`Array`) es una lista ordenada de valores de cualquier tipo, indexada desde 0, que crece y se encoge sola. En JavaScript los arreglos son objetos especiales con una propiedad `length` y muchos métodos en `Array.prototype` para agregar, quitar, buscar, transformar y ordenar.

### Cómo funciona: mutar o no mutar
La distinción más importante es si el método **modifica el arreglo original** o **devuelve uno nuevo**. En React, en reductores o al compartir datos entre módulos, mutar por accidente causa errores difíciles de rastrear.

| Mutan el original | No mutan (devuelven algo nuevo) |
|---|---|
| `push`, `pop` (final) | `slice`, `concat` |
| `shift`, `unshift` (inicio) | `map`, `filter`, `reduce`, `flatMap` |
| `splice` (insertar/quitar en medio) | `toSpliced` |
| `sort` | `toSorted` |
| `reverse` | `toReversed` |
| `fill`, `copyWithin` | `with(i, valor)` |

`toSorted`, `toReversed`, `toSpliced` y `with` llegaron en ES2023 como versiones que copian. Si el código debe correr en navegadores muy viejos, verifica soporte.

### `slice` vs `splice`
- **`slice(inicio, fin)`**: **copia** una porción desde `inicio` hasta `fin` (sin incluirlo). No toca el original. Índices negativos cuentan desde el final. `arr.slice()` copia todo (copia superficial).
- **`splice(inicio, cuántos, ...nuevos)`**: **modifica** el original: quita `cuántos` elementos desde `inicio`, inserta los nuevos y **devuelve los eliminados**.

```js
const a = [10, 20, 30, 40];
a.slice(1, 3);        // [20, 30]   a sigue igual
a.splice(1, 2, 99);   // [20, 30]   a ahora es [10, 99, 40]
```

### Acceso y búsqueda
- **Último elemento**: `arr.at(-1)` (ES2022) o `arr[arr.length - 1]`. `at` acepta índices negativos; `arr[-1]` no funciona (busca una propiedad llamada "-1" y da `undefined`).
- `includes(x)`: ¿está? `indexOf(x)`: posición o −1.
- `find`, `findIndex`, `findLast`, `some`, `every`: ver [[find, some, every y flatMap]].

### Crear arreglos
```js
Array(5).fill(0);                       // [0, 0, 0, 0, 0]
Array.from({ length: 5 }, (_, i) => i); // [0, 1, 2, 3, 4]
[...Array(5).keys()];                   // [0, 1, 2, 3, 4]
Array.from('hola');                     // ['h', 'o', 'l', 'a']
Array.from(document.querySelectorAll('li')); // NodeList → arreglo real
```

Cuidado: `Array(5)` crea 5 **huecos** (no `undefined`), y `map` se salta los huecos; por eso `Array(5).map((_, i) => i)` no funciona. `Array(3).fill([])` pone **el mismo arreglo** en las tres posiciones; para arreglos independientes usa `Array.from({ length: 3 }, () => [])`.

### Ejemplo
Manejo de renglones de un pedido sin mutar el estado original (útil en React):

```js
const renglones = [
  { sku: 'A1', cant: 2, precio: 50 },
  { sku: 'B2', cant: 1, precio: 120 },
];

// Agregar
const conNuevo = [...renglones, { sku: 'C3', cant: 3, precio: 10 }];

// Quitar el renglón B2
const sinB2 = renglones.filter(r => r.sku !== 'B2');

// Cambiar la cantidad del índice 0
const editado = renglones.with(0, { ...renglones[0], cant: 5 });

// Ordenar por precio sin tocar el original
const porPrecio = renglones.toSorted((x, y) => x.precio - y.precio);

const total = renglones.reduce((s, r) => s + r.cant * r.precio, 0); // 220
```

Las transformaciones se profundizan en [[map, filter y reduce]], el orden en [[sort]] y la copia en [[Spread y rest]].

### Cuándo usarlo y cuándo no
Usa arreglos para colecciones ordenadas. Si necesitas búsquedas por clave frecuentes, usa un `Map`; si necesitas valores únicos, un `Set` (ver [[Map y Set]]). Para datos numéricos masivos (imágenes, audio), existen los *typed arrays* (`Uint8Array`, `Float64Array`).

### Errores comunes
- **Mutar sin querer** con `sort` o `reverse` un arreglo que viene de props o de otro módulo.
- **Confundir `slice` y `splice`**.
- **`arr[-1]`** esperando el último elemento.
- **`Array(n).fill(objeto)`** compartiendo la misma referencia.
- **Comprobar vacío con `if (arr)`**: un arreglo vacío es truthy. Usa `arr.length === 0`.

### Para profundizar
- [`Array` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
- [Colecciones indexadas (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Indexed_collections)
- [`Array.prototype.with()` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/with)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué métodos de arreglo mutan el original?
> `push`, `pop`, `shift`, `unshift`, `splice`, `sort`, `reverse`, `fill`. Sus versiones que devuelven copia: `toSorted`, `toReversed`, `toSpliced`, `with`.

> [!question]- ¿Qué diferencia hay entre `slice` y `splice`?
> `slice(inicio, fin)` devuelve una copia del tramo sin tocar el original. `splice(inicio, cuántos, ...nuevos)` quita/inserta **en el original** y devuelve lo quitado.

> [!question]- ¿Cómo obtienes el último elemento?
> `arr.at(-1)` (o `arr[arr.length - 1]`).

> [!question]- ¿Cómo creas un arreglo de 5 ceros o de 0 a 4?
> `Array(5).fill(0)` y `Array.from({ length: 5 }, (_, i) => i)`.

