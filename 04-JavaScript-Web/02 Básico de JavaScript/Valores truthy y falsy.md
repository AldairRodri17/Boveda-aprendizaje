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
# Valores truthy y falsy

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
En JavaScript, cualquier valor se puede usar donde se espera un booleano (`if`, `while`, `!`, `&&`, `||`, el ternario). El valor se convierte a `true` o `false` según una regla fija: los valores que se convierten a `false` se llaman **falsy** y **todos los demás** son **truthy**.

### Cómo funciona
La lista de falsy es corta y cerrada. Apréndela de memoria:

| Valor falsy | Nota |
|---|---|
| `false` | |
| `0`, `-0` | el cero numérico |
| `0n` | el cero `BigInt` |
| `''` | la cadena **vacía** (sin ningún carácter) |
| `null` | |
| `undefined` | |
| `NaN` | |

(Existe un caso exótico más, `document.all`, por compatibilidad histórica del navegador.)

**Todo lo demás es truthy**, incluidos valores que "parecen vacíos":

```js
!!'0'      // true   cadena con un carácter
!!' '      // true   cadena con un espacio
!!'false'  // true   cadena no vacía
!![]       // true   arreglo vacío: es un objeto
!!{}       // true   objeto vacío
!!-1       // true
!!Infinity // true
!!''       // false
!!NaN      // false
```

### `!!x`
`!` convierte a booleano y niega; aplicarlo dos veces convierte **sin negar**. `!!x` equivale a `Boolean(x)`. Úsalo cuando necesitas un booleano real (por ejemplo, para guardar una bandera o devolver de una función `esX()`); en un `if` no hace falta, porque el `if` ya convierte.

```js
const tieneNotas = !!pedido.notas;   // true o false
Boolean(pedido.notas);               // lo mismo, más explícito
```

### El error de `if (cantidad)`
```js
function validar(cantidad) {
  if (!cantidad) return 'Captura una cantidad';  // problema
  return null;
}
validar(0);   // 'Captura una cantidad'  ← pero 0 puede ser válido (p. ej., "sin existencias")
validar('0'); // null ← la cadena '0' es truthy aunque represente cero
validar(NaN); // 'Captura una cantidad' (correcto, pero por casualidad)
```

`if (cantidad)` mezcla "no hay valor" con "el valor es cero" y además deja pasar cadenas. Sé explícito con lo que significa "válido":

```js
function validar(valorInput) {
  if (valorInput.trim() === '') return 'Captura una cantidad';
  const n = Number(valorInput);
  if (!Number.isInteger(n) || n < 0) return 'La cantidad debe ser un entero ≥ 0';
  return null;
}
```

Lo mismo con **arreglos vacíos**: `if (pedidos)` es siempre verdadero para un arreglo; para "hay pedidos" usa `if (pedidos.length > 0)`. Y con **objetos vacíos**: `Object.keys(obj).length === 0`.

### Relación con otros operadores
- `a || b` devuelve `b` si `a` es falsy, por eso reemplaza `0` y `''`; `a ?? b` solo reemplaza `null`/`undefined` (ver [[Operadores de null y lógicos en JS]]).
- `filter(Boolean)` quita **todos** los falsy de un arreglo: `[0, 1, '', 'a', null].filter(Boolean)` → `[1, 'a']`. Útil, pero también borra los ceros.
- Truthy/falsy **no** es lo mismo que `== true`: `'2' == true` es `false` (por coerción numérica) aunque `'2'` sea truthy. Ver [[== vs ===]].

### Ejemplo
Renderizado condicional en Itzamna.Clio (React):

```jsx
// Mal: si pendientes es 0, React pinta "0" en pantalla
{pendientes && <Badge>{pendientes}</Badge>}

// Bien: condición booleana explícita
{pendientes > 0 && <Badge>{pendientes}</Badge>}

// Texto opcional: aquí sí quieres tratar '' como "sin valor"
<p>{cliente.alias || cliente.nombre}</p>
```

### Dónde ocurre la conversión
La conversión a booleano ocurre en: la condición de `if`, `while`, `do...while` y `for`; el operador ternario `? :`; los operadores `!`, `&&` y `||` (que deciden con truthy/falsy aunque devuelvan el operando original); y en métodos como `filter`, `some`, `every` y `find`, que tratan el valor que devuelve tu callback como truthy o falsy. Por eso `pedidos.filter(p => p.notas)` conserva los pedidos con notas no vacías: el callback devuelve una cadena y el método la interpreta. No ocurre en comparaciones con `==` ni `===`, que siguen sus propias reglas.

### Cuándo usarlo y cuándo no
Confía en truthy/falsy para comprobar **presencia de objetos** (`if (usuario)`) o de cadenas cuando la vacía no es válida. Sé explícito con **números, booleanos que pueden ser `false` a propósito y arreglos**: compara con lo que realmente quieres (`=== undefined`, `> 0`, `.length`).

### Errores comunes
- **`if (cantidad)`** con números donde 0 es válido.
- **Creer que `[]` o `{}` son falsy**.
- **Creer que `'0'` o `'false'` son falsy**: son cadenas no vacías.
- **`&&` con números en JSX** que pinta un `0`.
- **`filter(Boolean)`** que borra ceros legítimos.

### Para profundizar
- [Truthy (glosario MDN)](https://developer.mozilla.org/en-US/docs/Glossary/Truthy)
- [Falsy (glosario MDN)](https://developer.mozilla.org/en-US/docs/Glossary/Falsy)
- [Conversión a booleano (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean#boolean_coercion)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Predice: `!!'0'`, `!!''`, `!![]`, `!!{}`, `!!NaN`, `!!' '`.
> `true`, `false`, `true`, `true`, `false`, `true`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cuáles son los valores falsy?
> `false`, `0`, `-0`, `0n`, `""`, `null`, `undefined` y `NaN`. Todo lo demás es truthy, incluidos `[]`, `{}` y `"0"`.

> [!question]- Error común: `if (cantidad)` para validar que hay un número. ¿Qué falla?
> Si la cantidad es `0` (válida), el `if` la trata como ausente. Compara explícito: `cantidad != null` o `Number.isFinite(cantidad)`.

> [!question]- ¿Cómo convierte `!!x`?
> Aplica `!` dos veces: devuelve el booleano equivalente de `x` (igual que `Boolean(x)`).

## Práctica

- [[Lab 2 - Predecir la salida de JavaScript (coerción, closures y this)]]

