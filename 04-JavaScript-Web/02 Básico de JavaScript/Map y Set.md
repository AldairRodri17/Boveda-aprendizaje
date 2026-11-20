---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: JavaScript y Web
tags: [javascript-web]
---
# Map y Set

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`Map` y `Set` son colecciones nativas de ES2015:

- **`Map`**: pares **clave → valor** donde la clave puede ser **de cualquier tipo** (objetos, números, funciones), conserva el **orden de inserción** y sabe su tamaño (`size`).
- **`Set`**: conjunto de **valores únicos**; agregar un valor repetido no hace nada.

Ambos son iterables y tienen búsquedas, inserciones y borrados rápidos (en promedio de tiempo constante).

### Cómo funciona
```js
const precios = new Map();
precios.set('A1', 50).set('B2', 120);   // set devuelve el Map: se puede encadenar
precios.get('A1');      // 50
precios.has('C3');      // false
precios.delete('B2');
precios.size;           // 1
for (const [sku, precio] of precios) console.log(sku, precio);

const etiquetas = new Set(['urgente', 'credito']);
etiquetas.add('urgente');  // ya existe: no cambia
etiquetas.has('credito');  // true
etiquetas.size;            // 2
```

**Igualdad de claves y valores**: se usa el algoritmo *SameValueZero*, que es como `===` salvo que **`NaN` es igual a `NaN`** (y `0` igual a `-0`). Por eso:

```js
[...new Set([1, 2, 2, '2', 3])];   // [1, 2, '2', 3]   → '2' y 2 son distintos
new Set([NaN, NaN]).size;          // 1
```

**Objetos como clave**: se comparan **por referencia**, no por contenido. Dos objetos literales con los mismos campos son **dos claves distintas**:

```js
const m = new Map();
m.set({ id: 1 }, 'a');
m.get({ id: 1 });         // undefined: es otro objeto
const k = { id: 1 };
m.set(k, 'b');
m.get(k);                 // 'b': misma referencia
```

Si necesitas "igualdad por valor", usa como clave un primitivo derivado (`cliente.id` o `` `${a}-${b}` ``).

### Quitar duplicados
```js
const unicos = [...new Set(arreglo)];          // o Array.from(new Set(arreglo))

// Por una propiedad (objetos): quedarse con el último de cada id
const porId = [...new Map(clientes.map(c => [c.id, c])).values()];
```

### Map vs objeto
Usa **`Map`** cuando:
- las claves **no son cadenas** (objetos, números que no quieres convertir a texto, nodos del DOM);
- agregas y quitas claves con frecuencia (diccionario dinámico, caché);
- necesitas `size` o el orden de inserción garantizado;
- las claves vienen del usuario: en un objeto, una clave como `__proto__` o `constructor` puede chocar con propiedades heredadas.

Usa un **objeto** cuando tienes una **estructura fija** con nombres conocidos (un registro, un DTO), cuando lo vas a serializar a JSON (un `Map` se serializa como `{}`), o para desestructurar.

Conversión: `Object.fromEntries(mapa)` y `new Map(Object.entries(obj))`.

### Ejemplo
Caché de clientes y agrupación en Itzamna.Clio:

```js
const cache = new Map();                    // id → promesa del cliente
export function obtenerCliente(id) {
  if (!cache.has(id)) {
    cache.set(id, fetch(`/api/clientes/${id}`).then(r => r.json()));
  }
  return cache.get(id);                     // llamadas repetidas reutilizan la promesa
}

// Agrupar pedidos por estado (ES2024)
const porEstado = Map.groupBy(pedidos, p => p.estado);
porEstado.get('nuevo');                     // arreglo de pedidos nuevos

// Selección de renglones en una tabla
const seleccion = new Set();
function alternar(id) { seleccion.has(id) ? seleccion.delete(id) : seleccion.add(id); }
```

Los `Set` también tienen (desde 2024 en los navegadores principales) métodos de conjuntos: `union`, `intersection`, `difference`, `symmetricDifference`, `isSubsetOf`. Revisa el soporte de tus navegadores objetivo antes de usarlos.

### Rendimiento y API en resumen
Las operaciones `get`, `set`, `has` y `delete` de un `Map` (y `add`, `has`, `delete` de un `Set`) son en promedio de **tiempo constante**, sin importar cuántos elementos haya. En cambio, `arr.includes(x)` o `arr.find(...)` recorren el arreglo: con 10 elementos da igual, pero si dentro de un bucle de 5,000 pedidos buscas en un arreglo de 5,000 clientes, haces millones de comparaciones. Convertir primero a `Map` o `Set` reduce el trabajo a una búsqueda directa por elemento.

Ambos se recorren **en orden de inserción** con `for...of`, `keys()`, `values()`, `entries()` y `forEach`. Para pasar a arreglo usa spread (`[...mapa.keys()]`) o `Array.from`. Un `Map` se construye desde cualquier iterable de pares (`new Map(Object.entries(obj))`) y un `Set` desde cualquier iterable de valores (`new Set('hola')` da `{'h','o','l','a'}`).

### Cuándo usarlo y cuándo no
`Map` y `Set` para colecciones dinámicas y búsquedas por clave; arreglos cuando importa el orden posicional o el acceso por índice; objetos para estructuras fijas. Si las claves son objetos y no quieres impedir que se liberen de memoria, usa [[WeakMap y WeakSet]].

### Errores comunes
- **Usar `mapa[clave] = valor`**: crea una propiedad del objeto `Map`, no una entrada; `get` no la ve.
- **Esperar igualdad por contenido** con objetos como clave.
- **Serializar un `Map` con `JSON.stringify`** y obtener `{}`.
- **Confundir `size` con `length`**.

### Para profundizar
- [`Map` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)
- [`Set` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)
- [Colecciones con clave (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Keyed_collections)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Predice: `[...new Set([1, 2, 2, '2', 3])]` y `new Set([NaN, NaN]).size`.
> `[1, 2, '2', 3]` y `1`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cuándo usar `Map` en lugar de un objeto?
> Cuando las claves no son strings (objetos, números), cuando agregas/quitas claves seguido, cuando necesitas el tamaño (`.size`) o el orden de inserción garantizado, y para evitar choques con propiedades heredadas.

> [!question]- ¿Cómo quitas duplicados de un arreglo?
> `[...new Set(arr)]`. `Set` compara con *SameValueZero* (como `===` pero `NaN` igual a `NaN`).

> [!question]- ¿Dos objetos literales iguales cuentan como la misma clave en un Map?
> No: se compara la referencia. `m.set({}, 1); m.set({}, 2)` deja dos entradas.

