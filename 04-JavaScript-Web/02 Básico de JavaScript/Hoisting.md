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
# Hoisting

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Hoisting** ("elevación") es el nombre informal de un comportamiento de JavaScript: antes de ejecutar un alcance (un script, un módulo, una función o un bloque), el motor **registra todas las declaraciones** que hay en él. Por eso un nombre "existe" desde el inicio de su alcance, aunque esté escrito más abajo. Lo que cambia según el tipo de declaración es **qué valor tiene** antes de llegar a su línea.

### Cómo funciona
La ejecución de un alcance tiene dos fases:
1. **Creación**: se recorre el código y se crean los enlaces (*bindings*) de cada declaración.
2. **Ejecución**: se corre línea por línea.

Lo que pasa en la fase de creación según la declaración:

| Declaración | Antes de su línea | Alcance |
|---|---|---|
| `function f() {}` | **Disponible completa** (se puede llamar) | función/bloque* |
| `var x = 1` | Existe y vale **`undefined`** | función |
| `let x = 1`, `const x = 1` | Existe pero **no inicializada** (TDZ): acceder lanza `ReferenceError` | bloque |
| `class C {}` | Igual que `let`: **TDZ** | bloque |
| `import` | Se resuelve antes de ejecutar el módulo | módulo |

\* Las declaraciones de función dentro de bloques tienen reglas especiales en modo no estricto; en módulos se comportan como de bloque. Evita declarar funciones dentro de `if`.

Las **expresiones de función** y las **flechas** asignadas a una variable siguen la regla de la variable (`var` → `undefined`, `let`/`const` → TDZ).

### Ejemplos
```js
console.log(a);   // undefined: var se eleva y se inicializa con undefined
var a = 1;

console.log(b);   // ReferenceError: Cannot access 'b' before initialization
let b = 2;

saludar();        // 'hola': la declaración completa está elevada
function saludar() { console.log('hola'); }

despedir();       // TypeError: despedir is not a function (vale undefined)
var despedir = function () {};

new Pedido();     // ReferenceError: la clase está en TDZ
class Pedido {}
```

### La zona muerta temporal (TDZ)
La **TDZ** (*Temporal Dead Zone*) es el tramo entre el **inicio del alcance** y la **línea de la declaración** de un `let`, `const` o `class`. En ese tramo la variable existe (por eso tapa a una del mismo nombre de afuera), pero leerla o escribirla lanza `ReferenceError`.

```js
const tasa = 0.16;
function calcular() {
  console.log(tasa);   // ReferenceError, no 0.16: la tasa local ya existe y está en TDZ
  const tasa = 0.08;
}
```

Es "temporal" porque depende del momento de ejecución, no de la posición: una función declarada antes puede usar una `const` de abajo si se **llama** después de que la `const` se inicializó.

```js
function mostrar() { return config.modo; } // define antes, no ejecuta
const config = { modo: 'prod' };
mostrar();                                // 'prod': ya pasó la TDZ
```

La TDZ existe para atrapar errores: con `var` el código del principio leería `undefined` en silencio; con `let`/`const` falla claramente.

### ¿Se elevan las clases?
Sí, en el sentido técnico: el nombre se registra al inicio del alcance. Pero **no se inicializan** hasta su línea, así que usar una clase antes de su declaración lanza `ReferenceError` (a diferencia de las declaraciones de función). En la práctica: declara las clases antes de usarlas.

### Ejemplo práctico
En un módulo de Itzamna.Clio, aprovechar la elevación de funciones para leer el archivo "de arriba hacia abajo":

```js
// pedidos-view.js
export function montar(raiz) {
  raiz.addEventListener('click', alClic);   // alClic está definida abajo: OK
  render(raiz);
}

function alClic(e) { /* ... */ }
function render(raiz) { /* ... */ }

const PLANTILLA = '<ul></ul>'; // úsala solo dentro de funciones que se llamen después de cargar el módulo
```

### Cuándo usarlo y cuándo no
No "uses" el hoisting como truco; entiéndelo para predecir errores. Reglas prácticas: declara con `const`/`let` (nunca `var`), declara antes de usar, y aprovecha la elevación de declaraciones de función solo para organizar funciones auxiliares debajo de la función principal. Relacionado: [[var, let y const]] y [[Alcance (scope)]].

### Errores comunes
- **Pensar que el código se "mueve" físicamente arriba**: nada se mueve; es la fase de creación.
- **Creer que `let`/`const` no se elevan**: sí se elevan, pero quedan en TDZ.
- **Llamar una expresión de función antes de asignarla**.
- **Sombrear una variable externa** y caer en su TDZ (el ejemplo de `tasa`).

### Para profundizar
- [Hoisting (glosario MDN)](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)
- [`let` y la zona muerta temporal (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#temporal_dead_zone_tdz)
- [Declaraciones de clase (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/class)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- ¿Qué imprime este código?
> ```js
> console.log(typeof f1, f1());
> function f1() { return 'ok'; }
> ```
>
> > [!success]- Respuesta
> > `function ok`.

> [!question]- ¿Qué imprime este código?
> ```js
> console.log(v1);
> var v1 = 1;
> ```
>
> > [!success]- Respuesta
> > `undefined`.

> [!question]- ¿Qué imprime este código?
> ```js
> f2();
> var f2 = function () {};
> ```
>
> > [!success]- Respuesta
> > `TypeError: f2 is not a function`: `f2` existe (vale `undefined`) pero aún no es función.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es el hoisting?
> Antes de ejecutar un alcance, JS registra sus declaraciones. Las `function` quedan disponibles completas; `var` se registra con `undefined`; `let`, `const` y `class` se registran pero quedan en zona muerta temporal hasta su línea.

> [!question]- ¿Qué es la zona muerta temporal (TDZ)?
> El tramo entre el inicio del bloque y la declaración de un `let`/`const`/`class`: acceder ahí lanza `ReferenceError`.

> [!question]- ¿Se elevan las clases?
> Se registran pero quedan en TDZ: usar `new MiClase()` antes de la declaración lanza `ReferenceError`.

## Práctica

- [[Lab 2 - Predecir la salida de JavaScript (coerción, closures y this)]]

