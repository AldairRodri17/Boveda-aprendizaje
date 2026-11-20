---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: JavaScript y Web
tags: [javascript-web]
---
# Clases en JS

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **clase** (`class`, desde ES2015) es una plantilla para crear objetos con el mismo estado y comportamiento. Es sintaxis más clara sobre el mecanismo de siempre de JavaScript, los prototipos (ver [[Prototipos y herencia prototípica]]): los métodos que declaras quedan en `Clase.prototype` y todas las instancias los comparten.

### Cómo funciona
```js
class Pedido {
  estado = 'nuevo';                  // campo público con valor inicial
  static #folio = 0;                 // estático privado (ver Campos privados)

  constructor(cliente, renglones = []) {
    this.cliente = cliente;
    this.renglones = renglones;
    this.folio = ++Pedido.#folio;
  }

  get total() {                      // getter: se usa como propiedad (p.total)
    return this.renglones.reduce((s, r) => s + r.cant * r.precio, 0);
  }

  agregar(renglon) { this.renglones.push(renglon); return this; }

  static desdeJson(json) {           // método estático: se llama sobre la clase
    return new Pedido(json.cliente, json.renglones);
  }
}

const p = new Pedido('Ferretería López').agregar({ cant: 2, precio: 50 });
p.total;                  // 100
Pedido.desdeJson({ cliente: 'X', renglones: [] });
```

- `constructor` se ejecuta con `new`. Sin `new`, llamar a una clase lanza `TypeError`.
- Los **campos** (`estado = 'nuevo'`; los privados con `#` se ven en [[Campos privados en clases JS]]) se crean en cada instancia antes de que corra el cuerpo del constructor (en clases derivadas, justo después de `super()`).
- Los **métodos** viven en el prototipo, una sola copia para todas las instancias.
- `get`/`set` definen propiedades calculadas.
- El cuerpo de la clase está siempre en **modo estricto**.
- Las clases no se pueden usar antes de su declaración (zona muerta temporal; ver [[Hoisting]]).

### `static`
Los miembros `static` pertenecen a **la clase**, no a las instancias: `Pedido.desdeJson()`, no `p.desdeJson()`. Se usan para fábricas, constantes, cachés compartidas o utilidades. Existen también los bloques `static { ... }` para inicialización compleja de estáticos.

### `extends` y `super`
`extends` crea una subclase cuyo prototipo encadena con el de la clase padre. `super` tiene dos usos:
- **`super(...)`** en el constructor llama al constructor del padre. En una subclase **debes llamarlo antes de usar `this`**; si no, `ReferenceError`.
- **`super.metodo()`** llama a la versión del padre desde un método sobrescrito.

```js
class PedidoMayoreo extends Pedido {
  constructor(cliente, renglones, descuento) {
    super(cliente, renglones);     // primero el padre
    this.descuento = descuento;
  }
  get total() {
    return super.total * (1 - this.descuento); // reutiliza el cálculo del padre
  }
}
new PedidoMayoreo('Y', [{ cant: 10, precio: 10 }], 0.1).total; // 90
```

Si la subclase no define constructor, se genera uno que pasa todos los argumentos a `super`.

### El error del método como callback
Los métodos **no están ligados** a su instancia. `this` depende de cómo se llama la función (ver [[this en JavaScript]]). Si pasas `p.agregar` como callback, se llama "suelto" y dentro `this` es `undefined` (la clase es estricta):

```js
class Contador {
  n = 0;
  inc() { this.n++; }
}
const c = new Contador();
boton.addEventListener('click', c.inc);  // this = el botón, no c → c.n no cambia
setTimeout(c.inc, 0);                    // TypeError: this es undefined

// Soluciones
boton.addEventListener('click', () => c.inc());   // flecha que llama con el objeto
boton.addEventListener('click', c.inc.bind(c));   // bind (ver call, apply y bind)

class Contador2 {
  n = 0;
  inc = () => { this.n++; };  // campo-flecha: ligado a la instancia
}
```

El campo-flecha es cómodo pero crea una función por instancia (no vive en el prototipo) y no se puede sobrescribir con `super.inc()`.

### Cuándo usarlo y cuándo no
Úsalas cuando tienes **estado + comportamiento** que van juntos y varias instancias: un cliente de API, un componente Web (`class X extends HTMLElement`, ver [[Web Components]]), errores propios (`class ErrorApi extends Error`). Para transformar datos, muchas veces bastan funciones puras y objetos literales; no todo necesita una clase. Prefiere composición sobre jerarquías profundas de `extends`.

### Errores comunes
- **Usar `this` antes de `super()`** en una subclase.
- **Pasar métodos como callback** sin ligar.
- **Olvidar `new`**: lanza `TypeError`.
- **Poner datos mutables en el prototipo** o en estáticos pensando que son por instancia.
- **Creer que son clases como en C#**: no hay sobrecarga de métodos ni `protected`, y los tipos no se verifican en ejecución (para eso, TypeScript).

### Para profundizar
- [Clases (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)
- [`extends` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/extends)
- [`super` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/super)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hacen `extends` y `super`?
> `extends` enlaza el prototipo de la subclase con el de la clase padre. En el constructor de la subclase debes llamar `super(...)` antes de usar `this`; `super.metodo()` llama al método del padre.

> [!question]- ¿Qué son los miembros `static`?
> Pertenecen a la clase, no a las instancias: `Clase.crear()`. Útiles para fábricas y constantes.

> [!question]- Error común: pasar un método de clase como callback. ¿Qué pasa?
> Pierde `this` (el cuerpo de clase es estricto, queda `undefined`). Usa un campo flecha `onClick = () => {...}` o `bind` en el constructor.

