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
# var, let y const

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`var`, `let` y `const` son las tres formas de declarar variables en JavaScript. `var` es la original; `let` y `const` llegaron en ES2015 para corregir sus problemas. Se diferencian en **alcance**, en **elevación** (hoisting), en si se pueden **reasignar** y **volver a declarar**, y en cómo se relacionan con el objeto global.

### Cómo funciona
| | `var` | `let` | `const` |
|---|---|---|---|
| Alcance | **Función** (ignora bloques) | **Bloque** `{ }` | **Bloque** `{ }` |
| Antes de su línea | Existe y vale `undefined` | **TDZ**: `ReferenceError` | **TDZ**: `ReferenceError` |
| Reasignar | Sí | Sí | **No** (`TypeError`) |
| Volver a declarar en el mismo alcance | Sí (en silencio) | No (`SyntaxError`) | No (`SyntaxError`) |
| Requiere valor inicial | No | No | **Sí** |
| Crea propiedad en `window` (script global) | Sí | No | No |
| Variable nueva por iteración en `for` | No (una sola) | **Sí** | Sí (en `for...of`) |

```js
if (true) {
  var a = 1;
  let b = 2;
}
a;   // 1: var "escapa" del bloque
b;   // ReferenceError: b no existe fuera del bloque

console.log(c); // undefined (var elevada)
var c = 3;
console.log(d); // ReferenceError (TDZ)
let d = 4;

const e = 5;
e = 6;          // TypeError: Assignment to constant variable
```

La elevación y la zona muerta temporal se explican en [[Hoisting]]; los niveles de alcance en [[Alcance (scope)]].

### Bucles y closures
La diferencia más famosa aparece con callbacks en bucles:

```js
for (var i = 0; i < 3; i++) setTimeout(() => console.log(i)); // 3, 3, 3
for (let j = 0; j < 3; j++) setTimeout(() => console.log(j)); // 0, 1, 2
```

Con `var` hay **una sola** `i` compartida; cuando corren los callbacks ya vale 3. Con `let`, cada vuelta tiene **su propia** `j`. Ver [[Closures]].

### `const` no es inmutabilidad
`const` impide **reasignar la variable** (que apunte a otra cosa), pero **no congela el valor**. Si el valor es un objeto o arreglo, su contenido se puede modificar:

```js
const renglones = [];
renglones.push({ sku: 'A1' });   // permitido: mismo arreglo, contenido distinto
renglones = [];                  // TypeError: reasignar no se puede

const config = { modo: 'dev' };
config.modo = 'prod';            // permitido
```

Para impedir cambios en el contenido usa `Object.freeze` (solo primer nivel; ver [[Objetos]]) o diseña con copias inmutables (`toSorted`, spread). En TypeScript, `readonly` y `as const` dan protección en compilación.

### Regla práctica
1. **Usa `const` por defecto.** La mayoría de las variables nunca se reasignan, y `const` lo comunica: quien lee sabe que ese nombre siempre apunta a lo mismo.
2. **Usa `let` solo cuando necesites reasignar**: contadores, acumuladores, variables que cambian en un bucle o según una condición.
3. **No uses `var`** en código nuevo. Sus únicos "beneficios" (alcance de función, redeclaración) son fuentes de errores. ESLint tiene las reglas `no-var` y `prefer-const` para aplicarlo automáticamente.

### Ejemplo
```js
// pedidos-service.js
const BASE = '/api/pedidos';              // nunca cambia

export async function cargarTodos() {
  const resultados = [];                  // el arreglo se llena, pero la variable no se reasigna
  let pagina = 1;                         // se reasigna: let
  let hayMas = true;

  while (hayMas) {
    const r = await fetch(`${BASE}?pagina=${pagina}`); // const dentro del bloque: una por vuelta
    const { datos, total } = await r.json();
    resultados.push(...datos);
    hayMas = resultados.length < total;
    pagina++;
  }
  return resultados;
}
```

### Cuándo usarlo y cuándo no
`const` para casi todo, `let` para lo que cambia, `var` nunca (salvo que mantengas código antiguo y no quieras cambiar su comportamiento). Al migrar código viejo, reemplazar `var` por `let` puede cambiar resultados si el código dependía del alcance de función o de la elevación; revisa cada caso.

### Errores comunes
- **Creer que `const` hace inmutable** un arreglo u objeto.
- **Usar `var` en bucles** con callbacks.
- **Acceder a `let`/`const` antes de su declaración** (TDZ).
- **Declarar `const` sin valor**: `const x;` es error de sintaxis.
- **Usar `let` "por si acaso"**: oculta qué variables realmente cambian.

### Para profundizar
- [`var` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var)
- [`let` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)
- [`const` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- ¿Qué imprime este código?
> ```js
> { var a = 1; let b = 2; }
> console.log(typeof a, typeof b);
> ```
>
> > [!success]- Respuesta
> > `number undefined`: `var` ignora el bloque; `b` no existe fuera de él (y `typeof` de algo no declarado da `"undefined"` sin error).

> [!question]- ¿Qué imprime este código?
> ```js
> const c = 1;
> c = 2;
> ```
>
> > [!success]- Respuesta
> > `TypeError: Assignment to constant variable.`

> [!question]- ¿Qué imprime este código?
> ```js
> console.log(x);
> let x = 5;
> ```
>
> > [!success]- Respuesta
> > `ReferenceError`: `x` está en la zona muerta temporal hasta su declaración.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencias hay entre `var`, `let` y `const`?
> `var`: alcance de función, se eleva inicializada en `undefined` y permite redeclarar. `let`/`const`: alcance de bloque y zona muerta temporal (TDZ). `const` no permite reasignar.

> [!question]- ¿`const` hace inmutable un arreglo?
> No. Impide reasignar la variable, pero el contenido se puede modificar (`arr.push(2)` funciona). Para congelar usa `Object.freeze` (superficial).

> [!question]- ¿Qué regla práctica seguir?
> `const` por defecto, `let` cuando necesites reasignar y nunca `var` en código nuevo.

