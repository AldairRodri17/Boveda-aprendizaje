---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: JavaScript y Web
tags: [javascript-web]
---
# WeakMap y WeakSet

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`WeakMap` y `WeakSet` son variantes de `Map` y `Set` (ver [[Map y Set]]) cuyas claves (o valores, en el caso de `WeakSet`) se guardan con **referencias débiles**: no impiden que el recolector de basura libere el objeto. Si la única referencia que queda a un objeto es la del `WeakMap`, el objeto se puede liberar y su entrada desaparece sola.

### Cómo funciona
Diferencias con `Map`:

| | `Map` | `WeakMap` |
|---|---|---|
| Tipo de clave | Cualquiera | Solo **objetos** (y símbolos no registrados) |
| Referencia a la clave | Fuerte (mantiene vivo el objeto) | **Débil** (no lo mantiene vivo) |
| Se puede recorrer | Sí (`for...of`, `keys`, `size`) | **No**: sin iteración ni `size` |
| Métodos | `get`, `set`, `has`, `delete`, `clear`, iteradores | Solo `get`, `set`, `has`, `delete` |

`WeakSet` es análogo: `add`, `has`, `delete`, solo objetos, sin recorrido.

```js
const datos = new WeakMap();
let nodo = document.querySelector('#pedido-1024');
datos.set(nodo, { cargado: Date.now() });
datos.get(nodo);       // { cargado: ... }

nodo.remove();         // se quita del DOM
nodo = null;           // ya nadie más lo referencia
// En algún momento el recolector libera el nodo y su entrada del WeakMap.
```

### Por qué no se puede recorrer
El recolector de basura decide **cuándo** liberar los objetos, y ese momento no es predecible ni observable desde el código. Si pudieras recorrer un `WeakMap` o leer su `size`, el resultado dependería de si el recolector ya pasó o no, y tu programa se volvería **no determinista** (el mismo código daría resultados distintos en cada ejecución). Para evitarlo, la especificación solo permite preguntar por una clave **que ya tienes en la mano**: si tienes la referencia, el objeto sigue vivo y la respuesta es estable.

### Para qué sirve
La idea general: **asociar datos a un objeto que no controlas o cuyo ciclo de vida es de otro**, sin alargarle la vida ni modificarlo.

1. **Metadatos de nodos del DOM**: guardar estado por elemento sin agregarle propiedades al nodo y sin fugar memoria cuando el nodo se elimina.
2. **Cachés por objeto** (memoización): el resultado calculado para un objeto se va con él.
3. **Datos privados por instancia** (patrón anterior a los campos `#`, ver [[Campos privados en clases JS]]).
4. **Marcar objetos ya procesados** con un `WeakSet`: detectar ciclos al recorrer un grafo, o evitar inicializar dos veces el mismo elemento.

### Ejemplo
Caché de totales calculados para pedidos, sin retenerlos en memoria cuando la vista los descarta:

```js
const cacheTotales = new WeakMap();

export function totalDe(pedido) {
  if (cacheTotales.has(pedido)) return cacheTotales.get(pedido);
  const total = pedido.renglones.reduce((s, r) => s + r.cant * r.precio, 0);
  cacheTotales.set(pedido, total);
  return total;
}

// Inicializar cada widget una sola vez aunque se llame varias veces
const inicializados = new WeakSet();
export function montarWidget(el) {
  if (inicializados.has(el)) return;
  inicializados.add(el);
  el.addEventListener('click', alClic);
}
```

Con un `Map` normal, `cacheTotales` mantendría vivos **todos** los pedidos que alguna vez pasaron por ahí, aunque la pantalla ya no los muestre: una [[Fugas de memoria|fuga de memoria]] clásica en aplicaciones de una sola página que corren horas.

Nota: la caché solo es correcta si los pedidos **no se mutan** después de calcular; si cambian, invalida la entrada con `delete`.

### Cuándo usarlo y cuándo no
Úsalo cuando la **clave es un objeto** con vida propia y quieres que la asociación muera con él. No lo uses si necesitas listar, contar o serializar las entradas (usa `Map`), ni con claves primitivas (cadenas, números: lanza `TypeError`). Para casos más avanzados existen `WeakRef` y `FinalizationRegistry`, pero son difíciles de usar bien y rara vez necesarios.

### Errores comunes
- **Usar primitivos como clave**: `TypeError: Invalid value used as weak map key`.
- **Esperar que la entrada desaparezca de inmediato**: depende del recolector; no programes lógica que dependa de ese momento.
- **Querer recorrerlo para depurar**: no se puede; usa un `Map` temporal mientras depuras.
- **Retener la clave en otro lado** (un arreglo, un closure) y creer que se liberará.

### Para profundizar
- [`WeakMap` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap)
- [`WeakSet` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakSet)
- [Gestión de memoria (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Memory_management)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia a `WeakMap` de `Map`?
> Las claves deben ser objetos y se guardan de forma **débil**: si nadie más referencia la clave, el recolector puede liberarla junto con su valor. No es iterable ni tiene `.size`.

> [!question]- ¿Para qué casos sirve?
> Asociar datos a objetos que no controlas sin evitar que se liberen: metadatos por nodo del DOM, cachés por objeto, datos privados por instancia.

> [!question]- ¿Por qué no puedes recorrer un WeakMap?
> Porque su contenido depende de cuándo corre el recolector de basura; listar sería no determinista.

