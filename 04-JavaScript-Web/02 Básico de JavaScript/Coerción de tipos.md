---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: JavaScript y Web
tags: [javascript-web]
---
# Coerción de tipos

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **coerción** es la conversión automática de un valor de un tipo a otro. Es **implícita** cuando JavaScript la hace solo porque un operador o una función espera otro tipo (`'3' - 1`), y **explícita** cuando tú la pides (`Number('3')`). La implícita es la causa de muchos resultados sorprendentes; conocer sus reglas te permite predecirlos y, sobre todo, evitarlos.

### Cómo funciona
Solo hay tres conversiones de destino: **a número**, **a cadena** y **a booleano**. Los objetos primero se convierten a **primitivo** (llamando `valueOf()` y luego `toString()`, o al revés si se prefiere cadena).

**A número** (`Number(x)`, `+x`, y operadores `- * / % **`, comparaciones `< >`):

| Valor | Número |
|---|---|
| `''`, `'   '` | `0` |
| `' 42 '` | `42` (se recortan espacios) |
| `'4a'` | `NaN` |
| `true` / `false` | `1` / `0` |
| `null` | `0` |
| `undefined` | `NaN` |
| `[]` | `0` (→ `''` → 0) |
| `[5]` | `5` (→ `'5'`) |
| `[1, 2]` | `NaN` (→ `'1,2'`) |

**A cadena** (`String(x)`, template literals, `+` con una cadena): `null` → `'null'`, `[1, [2, 3]]` → `'1,2,3'` (los arreglos se aplanan con comas), `{}` → `'[object Object]'`.

**A booleano** (`if`, `!`, `&&`, `||`): ver [[Valores truthy y falsy]].

### El operador `+`
`+` es especial porque suma **o** concatena. Regla: convierte ambos operandos a primitivo; si **alguno es cadena**, concatena; si no, suma numéricamente.

```js
1 + '2'        // '12'   hay cadena → concatena
'3' - 1        // 2      '-' solo resta → convierte a número
'5' * '2'      // 10
true + 1       // 2      true → 1
[] + []        // ''     ambos → '' → concatena
[] + {}        // '[object Object]'
null + 1       // 1      null → 0
undefined + 1  // NaN    undefined → NaN
```

El `+` unario (`+x`) siempre convierte a número: `+''` → 0, `+' 42 '` → 42, `+'4a'` → NaN.

### `Number()` vs `parseInt()`
- **`Number(x)`** convierte el valor **completo**: si sobra algo no numérico, da `NaN`. `Number('42px')` → NaN, `Number('')` → 0.
- **`parseInt(x, base)`** lee dígitos **desde el inicio** hasta el primer carácter inválido y descarta decimales: `parseInt('42px', 10)` → 42, `parseInt('3.9', 10)` → 3, `parseInt('', 10)` → NaN. Pasa **siempre** la base (10).
- **`parseFloat(x)`** igual, pero con decimales.

Elige `Number()` para validar entradas completas (un input numérico) y `parseInt` cuando quieres extraer el número inicial de un texto como `'16px'`.

### El caso `['1','2','3'].map(parseInt)`
Resultado: `[1, NaN, NaN]`. `map` llama al callback con **tres argumentos** `(valor, índice, arreglo)`, y `parseInt` usa el segundo como **base**: `parseInt('1', 0)` (base 0 = automática → 1), `parseInt('2', 1)` (base 1 inválida → NaN), `parseInt('3', 2)` (el 3 no existe en binario → NaN). Solución: `.map(Number)` o `.map(s => parseInt(s, 10))`.

### Ejemplo: sumar inputs
`input.value` **siempre es una cadena**. Con "2" y "3":

```js
const a = document.querySelector('#cant1').value; // "2"
const b = document.querySelector('#cant2').value; // "3"
a + b;                          // "23"  ← error clásico

Number(a) + Number(b);          // 5
document.querySelector('#cant1').valueAsNumber;   // 2 (en <input type="number">)

// Validación robusta
const n = Number(a);
if (a.trim() === '' || !Number.isFinite(n)) mostrarError('Cantidad inválida');
```

`valueAsNumber` devuelve `NaN` si el campo está vacío, lo cual facilita validar.

### Cuándo usarlo y cuándo no
Haz **coerción explícita** en los bordes del sistema (inputs, parámetros de URL, `localStorage`, CSV) y trabaja con tipos correctos dentro. Evita depender de la implícita en comparaciones: usa `===` (ver [[== vs ===]]). TypeScript ayuda mucho a detectar mezclas de tipos antes de ejecutar.

### Errores comunes
- **Concatenar en lugar de sumar** valores de formularios.
- **`parseInt` sin base** o usado en `map`.
- **`Number('')` da 0**: un campo vacío pasa como "cero" si no lo validas aparte.
- **Comparar cadenas numéricas** con `<`: `'10' < '9'` es `true` (compara texto). Convierte antes.

### Para profundizar
- [Coerción de tipos (glosario MDN)](https://developer.mozilla.org/en-US/docs/Glossary/Type_coercion)
- [`Number()` y conversión a número (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number#number_coercion)
- [`parseInt()` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Predice: `1 + '2'`, `'3' - 1`, `'5' * '2'`, `true + 1`.
> `"12"`, `2`, `10`, `2`.

> [!question]- Predice: `[] + []`, `[] + {}`, `null + 1`, `undefined + 1`.
> `""`, `"[object Object]"`, `1`, `NaN`.

> [!question]- Predice: `+''`, `+' 42 '`, `+'4a'`, `Number(null)`, `Number(undefined)`.
> `0`, `42`, `NaN`, `0`, `NaN`.

> [!question]- Predice: `Number([])`, `Number([5])`, `Number([1,2])`, `String([1,[2,3]])`.
> `0`, `5`, `NaN`, `"1,2,3"` (el arreglo pasa a string primero).

> [!question]- Predice: `['1','2','3'].map(parseInt)`.
> `[1, NaN, NaN]`: `map` pasa (valor, índice), así que llama `parseInt('2', 1)` y `parseInt('3', 2)`. Usa `.map(Number)`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es la coerción implícita?
> Que JS convierta un valor a otro tipo automáticamente según el operador: `+` con un string concatena; `-`, `*`, `/` convierten a número; `if` convierte a booleano.

> [!question]- ¿Cómo conviertes explícitamente a número y qué diferencia hay entre `Number()` y `parseInt()`?
> `Number('12px')` → `NaN` (todo el texto debe ser número). `parseInt('12px', 10)` → `12` (lee hasta donde puede). Pasa siempre la base a `parseInt`.

> [!question]- Error común: sumar valores de inputs. ¿Qué da `input1.value + input2.value` con "2" y "3"?
> `"23"`: `value` siempre es string. Convierte antes (`Number(...)` o `input.valueAsNumber`).

## Práctica

- [[Lab 2 - Predecir la salida de JavaScript (coerción, closures y this)]]

