---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: JavaScript y Web
tags: [javascript-web]
---
# == vs ===

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
JavaScript tiene dos operadores de igualdad:

- **`===` (igualdad estricta)**: compara **tipo y valor** sin convertir nada. Si los tipos son distintos, el resultado es `false`.
- **`==` (igualdad débil o abstracta)**: si los tipos son distintos, primero **convierte** uno o ambos operandos siguiendo un algoritmo de coerción y luego compara.

Sus negaciones son `!==` y `!=`. Hay un tercer mecanismo, `Object.is()`, que es como `===` pero con dos excepciones en casos numéricos raros.

### Cómo funciona `==`
El algoritmo (*IsLooselyEqual* en la especificación) se resume así:

1. Si los tipos son iguales, se comporta como `===`.
2. `null == undefined` es `true`, y **ninguno de los dos es igual a nada más** (ni a `0`, ni a `''`, ni a `false`).
3. Número vs cadena: la cadena se convierte a número (`'1' == 1` → `1 == 1` → `true`; `0 == ''` → `0 == 0` → `true`).
4. Booleano vs otra cosa: el booleano se convierte a número primero (`true` → 1, `false` → 0). Por eso `'1' == true` es `true` pero `'2' == true` es `false`.
5. Objeto vs primitivo: el objeto se convierte a primitivo (`valueOf`/`toString`). `[1] == '1'` → `'1' == '1'` → `true`; `[] == 0` → `'' == 0` → `true`.
6. Dos objetos solo son iguales si son **la misma referencia** (con `==` y con `===`).

Las reglas de conversión se detallan en [[Coerción de tipos]].

### Tabla de casos
| Expresión | Resultado | Motivo |
|---|---|---|
| `0 == ''` | `true` | `''` → 0 |
| `null == undefined` | `true` | regla especial |
| `null == 0` | `false` | `null` solo es igual a `undefined` |
| `NaN == NaN` | `false` | `NaN` nunca es igual a nada |
| `'1' == 1` | `true` | `'1'` → 1 |
| `[1] == '1'` | `true` | `[1]` → `'1'` |
| `null === undefined` | `false` | tipos distintos |
| `0 === -0` | `true` | `===` no distingue el signo del cero |
| `Object.is(0, -0)` | `false` | `Object.is` sí lo distingue |
| `Object.is(NaN, NaN)` | `true` | `Object.is` considera iguales los `NaN` |

### NaN
`NaN` (*Not a Number*) es el único valor que no es igual a sí mismo. Para detectarlo:
- `Number.isNaN(x)`: `true` solo si `x` es el valor `NaN`. Es la forma correcta.
- `isNaN(x)` global: convierte primero a número, así que `isNaN('hola')` da `true` aunque `'hola'` no sea `NaN`. Evítalo.
- `Object.is(x, NaN)` también funciona.
- `[NaN].includes(NaN)` es `true` (usa *SameValueZero*), pero `[NaN].indexOf(NaN)` es `-1` (usa `===`).

### Ejemplo
```js
// Validar el id que llega en la URL antes de pedir el pedido a la API
const id = new URLSearchParams(location.search).get('id'); // siempre string o null

if (id === null) {
  mostrarError('Falta el id');
} else if (Number(id) === 1024) {   // convierte explícitamente y compara estricto
  resaltar();
}

// Único uso idiomático de ==: null o undefined a la vez
function nombreVisible(cliente) {
  if (cliente.alias == null) return cliente.nombre; // alias es null o undefined
  return cliente.alias;
}
// Hoy se escribe más claro con ??: cliente.alias ?? cliente.nombre
```

### Cuándo usarlo y cuándo no
**Usa `===` siempre** por defecto: el resultado es predecible y no depende de reglas de conversión. El único uso de `==` que muchos equipos aceptan es `x == null` para preguntar "¿es `null` o `undefined`?" en una sola comparación; aun así, el operador `??` y el encadenamiento opcional `?.` (ver [[Operadores de null y lógicos en JS]]) cubren casi todos esos casos. Si lo usas, que sea intencional y consistente; ESLint tiene la regla `eqeqeq` con la opción `"smart"` o `{ "null": "ignore" }` para permitir solo ese caso.

Usa `Object.is` cuando necesites distinguir `-0` o tratar `NaN` como igual a sí mismo (por ejemplo, React lo usa para decidir si un estado cambió).

### Errores comunes
- **Comparar valores de inputs con números usando `==`**: funciona "por suerte" hasta que llega `''` (que es `== 0`). Convierte explícito con `Number()` y usa `===`.
- **`x === NaN`** para detectar `NaN`: siempre es `false`. Usa `Number.isNaN`.
- **Comparar objetos o arreglos por contenido** con `===`: `[1] === [1]` es `false`. Compara campo por campo o serializa con cuidado.
- **`if (x == true)`**: solo es verdadero si `x` se convierte a 1. Usa `if (x)` (ver [[Valores truthy y falsy]]) o `x === true`.

### Para profundizar
- [Comparaciones de igualdad e identidad (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness)
- [`Object.is()` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is)
- [IsLooselyEqual en ECMAScript](https://tc39.es/ecma262/#sec-islooselyequal)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Predice: `0 == ''`, `null == undefined`, `null == 0`, `NaN == NaN`.
> `true`, `true`, `false`, `false`.

> [!question]- Predice: `'1' == 1`, `[1] == '1'`, `null === undefined`.
> `true`, `true` (el arreglo pasa a "1"), `false`.

> [!question]- Predice: `Object.is(NaN, NaN)`, `Object.is(0, -0)`, `0 === -0`.
> `true`, `false`, `true`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre `==` y `===`?
> `===` compara sin convertir: tipo y valor deben coincidir. `==` aplica coerción antes de comparar, con reglas difíciles de recordar.

> [!question]- ¿Hay algún uso aceptado de `==`?
> `x == null`, que es verdadero solo para `null` y `undefined`. Fuera de eso, usa `===`.

> [!question]- ¿Cómo compruebas si un valor es `NaN`?
> `Number.isNaN(x)` u `Object.is(x, NaN)`. `NaN === NaN` es `false`.

## Práctica

- [[Lab 2 - Predecir la salida de JavaScript (coerción, closures y this)]]

