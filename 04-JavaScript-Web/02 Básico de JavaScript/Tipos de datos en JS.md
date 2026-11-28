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
# Tipos de datos en JS

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
JavaScript es un lenguaje de **tipado dinámico**: las variables no tienen tipo, los **valores** sí, y una misma variable puede guardar un número y luego una cadena. Hay **siete tipos primitivos** y un tipo **objeto**. Conocerlos explica cómo se copian, cómo se comparan y por qué aparecen resultados como `typeof null === 'object'`.

### Los tipos
**Primitivos** (inmutables, se comparan por valor):

| Tipo | Ejemplos | `typeof` |
|---|---|---|
| `undefined` | `undefined` (variable sin valor, propiedad inexistente) | `'undefined'` |
| `null` | `null` (ausencia intencional) | `'object'` (error histórico) |
| `boolean` | `true`, `false` | `'boolean'` |
| `number` | `42`, `3.14`, `NaN`, `Infinity`, `-0` | `'number'` |
| `bigint` | `10n`, `2n ** 64n` | `'bigint'` |
| `string` | `'hola'`, `` `plantilla` `` | `'string'` |
| `symbol` | `Symbol('id')` | `'symbol'` |

**Objetos** (mutables, se comparan por referencia): objetos literales, arreglos, funciones, `Date`, `Map`, `RegExp`, etc. `typeof` da `'object'` para todos salvo las funciones, que dan `'function'`.

Casos curiosos de `typeof`:
- `typeof null` → `'object'`: un error de la primera implementación que no se puede corregir sin romper la web. Para detectar `null` usa `x === null`.
- `typeof NaN` → `'number'`: `NaN` es un valor numérico especial ("resultado no numérico").
- `typeof []` → `'object'`: usa `Array.isArray(x)`.
- `typeof function () {}` → `'function'`.
- `typeof 10n` → `'bigint'`.

### `number`: doble precisión
Todos los `number` son **flotantes de 64 bits IEEE 754** (como `double` en C#). Consecuencias:

**Decimales inexactos.** `0.1` y `0.2` no tienen representación binaria exacta (igual que 1/3 no la tiene en decimal), así que `0.1 + 0.2` da `0.30000000000000004` y `0.1 + 0.2 === 0.3` es `false`. Para comparar usa una tolerancia: `Math.abs(a - b) < Number.EPSILON`. Para **dinero**, trabaja en **centavos enteros** (`12550` en lugar de `125.50`) o deja los cálculos al backend con `decimal`; para mostrar, `Intl.NumberFormat` (ver [[Intl (formato de números y fechas)]]).

**Enteros seguros.** Solo los enteros entre `-(2**53 - 1)` y `2**53 - 1` se representan exactamente. `Number.MAX_SAFE_INTEGER` es `9007199254740991`. Más allá, se pierden unidades: `2**53 + 1` da `9007199254740992` (igual que `2**53`). Por eso un `bigint` de SQL Server con valores grandes debe viajar como **cadena** en JSON, o usarse `BigInt` en JS. `Number.isSafeInteger(x)` lo comprueba.

**`bigint`** representa enteros de tamaño arbitrario (`10n`), pero no se mezcla con `number` en operaciones (`1n + 1` lanza `TypeError`) y no se serializa con `JSON.stringify`.

### Primitivos vs objetos al pasar a funciones
JavaScript siempre pasa **por valor**, pero con objetos el "valor" es una **referencia**:

```js
function cambiar(n, obj, arr) {
  n = 99;              // cambia la copia local; el original no
  obj.estado = 'listo';// muta el objeto compartido: el original SÍ cambia
  arr = [];            // reasigna la variable local; el original no cambia
}
let num = 1;
const pedido = { estado: 'nuevo' };
const lista = [1, 2];
cambiar(num, pedido, lista);
num;             // 1
pedido.estado;   // 'listo'
lista;           // [1, 2]
```

- Un **primitivo** se copia: la función no puede alterar la variable de quien llama.
- Un **objeto** comparte la referencia: la función puede **mutar** su contenido, pero **reasignar** el parámetro no afecta al de fuera.

Por eso en React no se muta el estado: si cambias un objeto in situ, la referencia sigue siendo la misma y React no detecta el cambio. Copia con spread o `structuredClone` (ver [[Spread y rest]]).

Los primitivos parecen tener métodos (`'hola'.toUpperCase()`) porque el motor los envuelve temporalmente en un objeto (`String`); no uses `new String('x')`.

### Ejemplo
Validar el tipo de lo que llega de la API antes de usarlo:

```js
function esPedidoValido(p) {
  return p !== null
    && typeof p === 'object'
    && typeof p.id === 'string'          // id bigint enviado como cadena
    && Number.isFinite(p.totalCentavos)  // número real, no NaN ni Infinity
    && Array.isArray(p.renglones);
}
const totalMostrado = new Intl.NumberFormat('es-MX', { style: 'currency', currency: 'MXN' })
  .format(pedido.totalCentavos / 100);
```

### Cuándo usarlo y cuándo no
Valida tipos en los **bordes** (datos de la API, formularios, `localStorage`); dentro del código, TypeScript te da esa seguridad en tiempo de compilación. Elige `null` para "sin valor intencional" y deja `undefined` para "no definido"; sé consistente con lo que espera tu API.

### Errores comunes
- **`typeof x === 'object'`** sin excluir `null`.
- **Comparar decimales con `===`**.
- **Dinero en flotantes**.
- **IDs de 64 bits como `number`**.
- **Creer que pasar un objeto a una función lo copia**.

### Para profundizar
- [Tipos y estructuras de datos (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Data_structures)
- [`typeof` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof)
- [`Number.MAX_SAFE_INTEGER` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Predice: `typeof null`, `typeof NaN`, `typeof []`, `typeof function(){}`, `typeof 10n`.
> `"object"` (error histórico), `"number"`, `"object"` (usa `Array.isArray`), `"function"`, `"bigint"`.

> [!question]- Predice: `2**53 + 1` y `Number.MAX_SAFE_INTEGER`.
> `9007199254740992` (perdió precisión) y `9007199254740991`. Arriba de eso usa `BigInt`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cuáles son los tipos primitivos de JS?
> `string`, `number`, `bigint`, `boolean`, `undefined`, `symbol` y `null`. Todo lo demás (arreglos, funciones, fechas) es `object`.

> [!question]- ¿Qué diferencia hay entre pasar un primitivo y un objeto a una función?
> Ambos se pasan por valor, pero el valor de un objeto es una **referencia**: si la función muta el objeto, el cambio se ve afuera; si reasigna el parámetro, no.

> [!question]- ¿Por qué `0.1 + 0.2 !== 0.3`?
> `number` es punto flotante IEEE 754 y 0.1 no se representa exacto; da `0.30000000000000004`. Para dinero usa enteros (centavos) o redondea al mostrar.

