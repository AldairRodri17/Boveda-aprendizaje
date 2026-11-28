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
# sort

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`arr.sort(comparador)` ordena los elementos de un arreglo **en el mismo arreglo** (lo modifica) y devuelve ese mismo arreglo. Sin comparador, ordena como texto; con comparador, tú decides el criterio. Desde ES2023 existe `toSorted()`, que hace lo mismo pero devuelve una **copia** ordenada.

### Cómo funciona
**Sin comparador**: convierte cada elemento a **cadena** y los ordena por los códigos de sus caracteres (unidades UTF-16). Con números da resultados "raros":

```js
[10, 1, 5, 100].sort();   // [1, 10, 100, 5]  → compara '10' vs '5' como texto
['b', 'a', 'C'].sort();   // ['C', 'a', 'b']  → las mayúsculas tienen códigos menores
```

Los `undefined` siempre se mandan al final.

**Con comparador** `(a, b) => número`:
- **negativo**: `a` va **antes** que `b`;
- **positivo**: `a` va **después** que `b`;
- **cero**: se consideran iguales y conservan su orden relativo.

El comparador debe ser **consistente**: para los mismos `a` y `b` dar siempre el mismo signo, y ser simétrico (si `cmp(a, b) < 0`, entonces `cmp(b, a) > 0`). Devolver un booleano (`(a, b) => a > b`) es un error clásico: `true`/`false` se convierten a 1/0 y nunca da negativo, así que el orden resultante no es confiable.

```js
[10, 1, 5, 100].sort((a, b) => a - b);  // [1, 5, 10, 100] ascendente
[10, 1, 5, 100].sort((a, b) => b - a);  // [100, 10, 5, 1] descendente
```

`a - b` funciona para números finitos; con `NaN` el resultado es inconsistente.

**Estabilidad**: desde ES2019 `sort` es **estable**: los elementos que el comparador considera iguales conservan su orden original. Eso permite ordenar por varios criterios en pasos, aunque un solo comparador combinado es más claro.

### Varios criterios
```js
const personas = [
  { nombre: 'Luis', edad: 30 },
  { nombre: 'Ana', edad: 30 },
  { nombre: 'Eva', edad: 25 },
];
personas.sort((a, b) =>
  b.edad - a.edad ||                           // 1º edad descendente
  a.nombre.localeCompare(b.nombre, 'es')        // 2º nombre ascendente si empatan
);
// Ana (30), Luis (30), Eva (25)
```

El `||` funciona porque si la diferencia de edades es `0` (falsy), pasa al segundo criterio.

### Texto en español
Ordenar cadenas con `<`/`>` o sin comparador falla con acentos y eñes: `['é', 'e', 'z', 'a'].sort()` da `['a', 'e', 'z', 'é']`, porque `é` tiene un código mayor que `z`. Usa comparación sensible al idioma:

```js
['é', 'e', 'z', 'a'].sort((a, b) => a.localeCompare(b, 'es'));  // ['a', 'e', 'é', 'z']

// Para listas grandes, crea un Collator una vez (más rápido que localeCompare repetido)
const col = new Intl.Collator('es', { sensitivity: 'base', numeric: true });
clientes.sort((a, b) => col.compare(a.nombre, b.nombre));
['folio10', 'folio9'].sort(col.compare);  // ['folio9', 'folio10'] gracias a numeric: true
```

`sensitivity: 'base'` trata "a", "á" y "A" como iguales en la comparación; `numeric: true` ordena los dígitos como números. Ver [[Intl (formato de números y fechas)]].

### `sort` muta
`sort` **modifica el arreglo original** y devuelve la misma referencia (no una copia). Si el arreglo viene de props de React, del estado o de una caché compartida, ordenarlo in situ provoca errores sutiles: otra parte del código ve el orden cambiado, o React no detecta el cambio porque la referencia es la misma.

```js
const ordenados = pedidos.toSorted((a, b) => b.total - a.total); // copia (ES2023)
const ordenados2 = [...pedidos].sort((a, b) => b.total - a.total); // alternativa compatible
```

### Ejemplo
Tabla de pedidos con orden por columna en Itzamna.Clio:

```js
const col = new Intl.Collator('es', { numeric: true });
const comparadores = {
  folio:   (a, b) => col.compare(a.folio, b.folio),
  cliente: (a, b) => col.compare(a.cliente, b.cliente),
  total:   (a, b) => a.totalCentavos - b.totalCentavos,
  fecha:   (a, b) => Date.parse(a.creadoEn) - Date.parse(b.creadoEn),
};

function ordenar(lista, campo, desc = false) {
  const cmp = comparadores[campo];
  return lista.toSorted(desc ? (a, b) => cmp(b, a) : cmp);
}
```

Si los datos vienen paginados del servidor, ordena en el servidor (`ORDER BY` en tu procedimiento de SQL Server): ordenar en el cliente solo reordena la página visible.

### Errores comunes
- **Ordenar números sin comparador**.
- **Comparador que devuelve booleano**.
- **Mutar arreglos compartidos** con `sort`.
- **Ordenar texto en español** sin `localeCompare` o `Intl.Collator`.
- **Restar cadenas** en el comparador (`a.folio - b.folio` con folios como texto da `NaN`).

### Para profundizar
- [`Array.prototype.sort()` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)
- [`Array.prototype.toSorted()` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toSorted)
- [`Intl.Collator` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/Collator)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Ordena `[10, 1, 5, 100]` numéricamente de menor a mayor.
> `arr.sort((a, b) => a - b)` → `[1, 5, 10, 100]`.

> [!question]- Ordena personas por edad descendente y, si empatan, por nombre ascendente.
> ```js
> personas.toSorted((a, b) => b.edad - a.edad || a.nombre.localeCompare(b.nombre));
> ```

> [!question]- ¿Cómo ordenar `['é','e','z','a']` en orden alfabético español?
> `arr.sort((a, b) => a.localeCompare(b, 'es'))` → `['a','e','é','z']`. Sin `localeCompare`, la `é` queda al final.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo ordena `sort()` sin comparador?
> Convierte a string y ordena por unidades de código UTF-16: `[10, 1, 5, 100].sort()` → `[1, 10, 100, 5]`.

> [!question]- ¿Qué debe devolver la función comparadora?
> Negativo si `a` va antes, positivo si `b` va antes, 0 si da igual. Para números: `(a, b) => a - b`.

> [!question]- ¿`sort` modifica el arreglo?
> Sí, ordena en el lugar y devuelve el mismo arreglo. Para una copia usa `toSorted()` o `[...arr].sort()`.

