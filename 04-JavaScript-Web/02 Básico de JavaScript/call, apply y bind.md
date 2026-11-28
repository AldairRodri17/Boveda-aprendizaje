---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: JavaScript y Web
tags: [javascript-web]
contenido: completo
---
# call, apply y bind

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`call`, `apply` y `bind` son métodos que tienen **todas las funciones** (viven en `Function.prototype`) y permiten **controlar el valor de `this`** con el que se ejecutan. Son la forma explícita de fijar `this` (ver [[this en JavaScript]]).

### Cómo funciona
| Método | Ejecuta la función | Cómo recibe los argumentos | Devuelve |
|---|---|---|---|
| `f.call(thisArg, a, b)` | **Sí, ahora** | Uno por uno | El resultado de `f` |
| `f.apply(thisArg, [a, b])` | **Sí, ahora** | En un **arreglo** (o similar) | El resultado de `f` |
| `f.bind(thisArg, a)` | **No** | Fija los primeros (aplicación parcial) | Una **función nueva** ligada |

Truco para recordar: **a**pply = **a**rreglo; **c**all = **c**omas.

```js
function presentar(saludo, signo) {
  return `${saludo}, soy ${this.nombre}${signo}`;
}
const ana = { nombre: 'Ana' };

presentar.call(ana, 'Hola', '!');      // 'Hola, soy Ana!'
presentar.apply(ana, ['Hola', '!']);   // 'Hola, soy Ana!'

const presentarAna = presentar.bind(ana, 'Qué tal'); // fija this y el primer argumento
presentarAna('?');                     // 'Qué tal, soy Ana?'
```

### `bind` es permanente
Una función ligada con `bind` tiene su `this` **fijo para siempre**: llamarla con `call`, `apply` u otro `bind` no lo cambia (solo se pueden agregar más argumentos):

```js
const f = presentar.bind({ nombre: 'otro' });
f.call({ nombre: 'x' }, 'Hola', '.');  // 'Hola, soy otro.'
f.bind({ nombre: 'y' })('Hola', '.');  // 'Hola, soy otro.'
```

La excepción es `new`: si usas una función ligada como constructor, `new` crea su propio `this`.

### ¿Y en una función flecha?
Las flechas **no tienen `this` propio**: lo toman del alcance donde se escribieron. Por eso `call`, `apply` y `bind` **no pueden cambiar** su `this`; el primer argumento se ignora. `bind` sobre una flecha solo sirve para fijar argumentos:

```js
const flecha = () => this;          // en un módulo, this es undefined
flecha.call({ x: 1 });              // undefined
const suma = (a, b) => a + b;
const mas10 = suma.bind(null, 10);  // aplicación parcial: válida
mas10(5);                           // 15
```

### `apply` en tiempos de spread
`apply` nació para llamar funciones con un arreglo de argumentos, por ejemplo `Math.max.apply(null, numeros)`. Hoy el **spread** lo hace más claro: `Math.max(...numeros)` (ver [[Spread y rest]]). `apply` sigue teniendo sentido cuando:
- necesitas **a la vez** fijar `this` y pasar un arreglo de argumentos de forma dinámica (envoltorios genéricos, decoradores): `return original.apply(this, args)`;
- mantienes código antiguo o trabajas con objetos *array-like* como `arguments`.

En código nuevo, `fn.call(this, ...args)` o `Reflect.apply(fn, this, args)` cubren lo mismo.

### Ejemplo
**Listeners que necesitan la instancia**:

```js
class ListaPedidos {
  constructor(raiz) {
    this.raiz = raiz;
    this.seleccion = new Set();
    this.alClic = this.alClic.bind(this);    // liga una vez y guarda la referencia
    raiz.addEventListener('click', this.alClic);
  }
  alClic(e) {
    const id = e.target.closest('[data-id]')?.dataset.id;
    if (id) this.seleccion.add(id);          // this es la instancia, no el elemento
  }
  destruir() {
    this.raiz.removeEventListener('click', this.alClic); // misma referencia: sí se quita
  }
}
```

Guardar la función ligada es importante: `removeEventListener(this.alClic.bind(this))` no quita nada, porque cada `bind` crea una **función nueva** distinta.

**Envoltorio que mide tiempo sin perder `this`**:

```js
function medido(fn) {
  return function (...args) {
    const t = performance.now();
    try { return fn.apply(this, args); }       // conserva el this de quien llame
    finally { console.log(fn.name, performance.now() - t, 'ms'); }
  };
}
```

### Cuándo usarlo y cuándo no
- `bind`: callbacks y listeners que necesitan un `this` fijo; aplicación parcial.
- `call`: invocar una función con un `this` concreto, por ejemplo métodos prestados (`Array.prototype.map.call(nodeList, fn)`), aunque hoy `Array.from` es más claro.
- `apply`: envoltorios genéricos.

Muchas veces una flecha (`() => obj.metodo()`) es más legible que `bind`.

### Errores comunes
- **Esperar que `call`/`bind` cambien el `this` de una flecha**.
- **Intentar re-ligar** una función ya ligada.
- **`removeEventListener` con un `bind` nuevo**.
- **Confundir `bind` con `call`**: `bind` no ejecuta; `boton.onclick = f.call(obj)` asigna el resultado, no una función.

### Para profundizar
- [`Function.prototype.call()` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call)
- [`Function.prototype.apply()` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)
- [`Function.prototype.bind()` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- ¿Qué imprime este código?
> ```js
> function sum(a, b) { return this.base + a + b; }
> console.log(sum.call({ base: 10 }, 1, 2));
> console.log(sum.apply({ base: 100 }, [1, 2]));
> console.log(sum.bind({ base: 1 }, 5)(5));
> ```
>
> > [!success]- Respuesta
> > `13`, `103`, `11`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre `call`, `apply` y `bind`?
> `call(thisArg, a, b)` y `apply(thisArg, [a, b])` invocan ya, con argumentos sueltos o en arreglo. `bind(thisArg, ...)` **no invoca**: devuelve una nueva función con `this` (y opcionalmente argumentos) fijados.

> [!question]- ¿Para qué sirve hoy `apply` si existe el spread?
> Casi siempre `f(...arr)` lo reemplaza (`Math.max(...nums)`). `apply` sigue apareciendo en código antiguo o cuando necesitas también fijar `this`.

> [!question]- ¿Funciona `bind` en una flecha?
> No cambia su `this` (las flechas no tienen); solo fija argumentos.

## Práctica

- [[Lab 2 - Predecir la salida de JavaScript (coerción, closures y this)]]

