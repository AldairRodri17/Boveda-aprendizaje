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
# Funciones (declaración, expresión y flecha)

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una función es un bloque de código reutilizable que recibe parámetros y devuelve un valor. En JavaScript las funciones son **valores de primera clase**: se guardan en variables, se pasan como argumentos (callbacks) y se devuelven desde otras funciones. Hay tres formas principales de escribirlas, y no son equivalentes.

### Las tres formas
```js
// 1. Declaración
function sumar(a, b) { return a + b; }

// 2. Expresión (anónima o con nombre)
const restar = function (a, b) { return a - b; };

// 3. Flecha (arrow function, ES2015)
const multiplicar = (a, b) => a * b;          // retorno implícito
const crear = (n) => ({ n, creado: Date.now() }); // objeto: va entre paréntesis
```

### Hoisting: declaración vs expresión
- Una **declaración** se eleva completa: puedes llamarla **antes** de la línea donde aparece en el código.
- Una **expresión** asignada a `const`/`let` sigue las reglas de la variable: antes de su línea está en la zona muerta temporal y llamarla lanza `ReferenceError`. Con `var`, la variable existe pero vale `undefined`, así que lanza `TypeError: ... is not a function`.

```js
saludar();                         // funciona
function saludar() { console.log('hola'); }

despedir();                        // ReferenceError
const despedir = () => console.log('adiós');
```

Detalle en [[Hoisting]].

### Diferencias de la función flecha
| | Función normal | Flecha |
|---|---|---|
| `this` | Depende de cómo se llama | **Léxico**: el del alcance donde se escribió |
| `arguments` | Sí | No (usa `...args`) |
| Se puede usar con `new` | Sí | No (`TypeError`) |
| Tiene `prototype` | Sí | No |
| Sintaxis | `function` + llaves | corta, retorno implícito |

La diferencia de `this` es la importante (ver [[this en JavaScript]]):
- **Ventaja**: dentro de un método, un callback flecha ve el `this` del método. `this.renglones.map(r => r.precio * this.tasa)` funciona.
- **Problema**: como **método de un objeto literal**, la flecha no recibe el objeto como `this`; toma el `this` de afuera (en un módulo, `undefined`):

```js
const carrito = {
  items: [1, 2],
  totalMal: () => this.items.length,      // TypeError: this es undefined en un módulo
  totalBien() { return this.items.length; } // sintaxis corta de método
};
```

Regla práctica: **métodos con sintaxis de método** (`nombre() {}`), **callbacks con flecha**.

### Parámetros por defecto y rest
```js
function paginar(lista, pagina = 1, tamano = 20) {
  const inicio = (pagina - 1) * tamano;
  return lista.slice(inicio, inicio + tamano);
}
paginar(datos);              // pagina 1, tamano 20
paginar(datos, undefined, 5);// pagina 1 (undefined activa el defecto), tamano 5
paginar(datos, null);        // pagina = null: null NO activa el defecto
```

El valor por defecto se aplica **solo cuando el argumento es `undefined`** (omitido o explícito). Se evalúa en cada llamada, así que `function f(lista = [])` crea un arreglo nuevo cada vez (a diferencia de Python). Un defecto puede usar parámetros anteriores: `function f(a, b = a * 2)`.

Los parámetros *rest* (`...args`) juntan los argumentos sobrantes en un arreglo real; ver [[Spread y rest]].

### Ejemplo
```js
// Declaración: utilidades del módulo, legibles y elevadas
export function calcularTotal(renglones, tasaIva = 0.16) {
  const subtotal = renglones.reduce((s, r) => s + r.cant * r.precio, 0); // flecha como callback
  return subtotal * (1 + tasaIva);
}

// Funciones de orden superior: reciben o devuelven funciones
const conLog = (fn) => (...args) => {
  console.time(fn.name);
  try { return fn(...args); } finally { console.timeEnd(fn.name); }
};
const totalMedido = conLog(calcularTotal);
```

### Cuándo usarlo y cuándo no
- **Declaración**: funciones con nombre de nivel de módulo; se leen bien y aparecen con nombre en las trazas de error.
- **Flecha**: callbacks (`map`, `then`, listeners), funciones cortas, y cuando necesitas el `this` exterior.
- **Expresión con `function`**: cuando necesitas `this` dinámico en un valor (raro hoy) o una función con nombre recursiva.
- **No uses flecha** para métodos de objeto, constructores, ni handlers que dependan de `this` como el elemento (`this` en `addEventListener` con función normal es el elemento).

### Errores comunes
- **Flecha como método de objeto** con `this`.
- **Devolver un objeto literal sin paréntesis** en una flecha: `() => { a: 1 }` devuelve `undefined` (las llaves se leen como bloque).
- **Llamar una expresión antes de definirla**.
- **Esperar que `null` active el parámetro por defecto**.

### Para profundizar
- [Funciones (guía MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions)
- [Funciones flecha (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
- [Parámetros por defecto (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencias tiene una función flecha frente a una normal?
> No tiene su propio `this`, `arguments` ni `prototype` (toma `this` del alcance donde se definió) y no se puede usar con `new`. Es ideal para callbacks.

> [!question]- ¿Qué diferencia de hoisting hay entre declaración y expresión?
> La declaración `function f(){}` se eleva completa: puedes llamarla antes de la línea. Una expresión `const f = function(){}` o flecha no existe hasta ejecutarse esa línea.

> [!question]- ¿Qué son los parámetros por defecto y cuándo se aplican?
> `function f(x = 10)`: el valor se usa cuando el argumento es `undefined` (no cuando es `null`).

> [!question]- Error común: usar flecha como método de un objeto literal. ¿Qué pasa?
> `const o = { n: 1, f: () => this.n }`: `this` no es `o` sino el `this` de afuera (en un módulo, `undefined`). Usa sintaxis de método `f() { return this.n }`.

