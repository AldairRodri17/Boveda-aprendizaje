---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: JavaScript y Web
aliases: ["Campos privados con #"]
tags: [javascript-web]
contenido: completo
---
# Campos privados en clases JS

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Desde ES2022, las clases de JavaScript pueden tener **campos y métodos privados** con el prefijo `#`: `#saldo`, `#validar()`. Son privados **de verdad**: el lenguaje impide leerlos o escribirlos desde fuera del cuerpo de la clase. Sirven para encapsular estado interno (un token, una caché, un contador) y garantizar invariantes, como lo harías con `private` en C#.

### Cómo funciona
```js
class Cuenta {
  #saldo = 0;                 // campo privado con valor inicial
  static #instancias = 0;     // campo privado estático

  constructor(titular) {
    this.titular = titular;   // campo público
    Cuenta.#instancias++;
  }

  depositar(monto) {
    this.#validar(monto);
    this.#saldo += monto;
  }

  get saldo() { return this.#saldo; } // solo lectura desde fuera

  #validar(monto) {           // método privado
    if (!(monto > 0)) throw new RangeError('El monto debe ser positivo');
  }

  static esCuenta(obj) { return #saldo in obj; } // comprobación de marca
}

const c = new Cuenta('Ana');
c.depositar(100);
c.saldo;        // 100
// c.#saldo;    // SyntaxError: no se puede ni escribir fuera de la clase
c['#saldo'];    // undefined: "#saldo" como cadena es otra propiedad pública
```

Puntos clave:
- El `#` **es parte del nombre**. `#saldo` y `saldo` son cosas distintas y pueden coexistir.
- Deben **declararse** en el cuerpo de la clase; no puedes crear `this.#nuevo` en un método si no lo declaraste.
- Acceder a un privado desde fuera es un **error de sintaxis** (falla antes de ejecutar). Si dentro de la clase accedes a `otro.#saldo` y `otro` no es una instancia, lanza `TypeError`.
- Los privados **no aparecen** en `Object.keys`, `JSON.stringify`, `for...in` ni en `{...obj}`.
- Las subclases **no** acceden a los privados del padre; deben usar métodos o *getters* públicos o protegidos por convención.

### Comprobar si un objeto tiene el privado (*brand check*)
`#campo in obj` (ES2022) devuelve `true` si `obj` fue construido por esta clase (tiene ese campo privado), sin lanzar error. Es útil en métodos estáticos o en comparaciones:

```js
Cuenta.esCuenta(c);             // true
Cuenta.esCuenta({ saldo: 1 });  // false
```

Es más fiable que `instanceof`, que se puede engañar cambiando prototipos y falla entre ventanas o iframes.

### Diferencia con la convención `_campo`
Antes de `#`, se usaba un guion bajo (`this._saldo`) para avisar "no toques esto". Es **solo una convención**: la propiedad es pública, cualquiera la lee y modifica, aparece en `JSON.stringify` y en el autocompletado. `#` lo hace **cumplir el lenguaje**.

Otra forma antigua de privacidad eran los [[Closures]] (variables en el constructor) o un `WeakMap` por instancia (ver [[WeakMap y WeakSet]]). `#` es más claro y eficiente.

### Ejemplo
Cliente de API para Xolotl.Hecate que guarda el token sin exponerlo:

```js
export class ClienteApi {
  #token = null;
  #baseUrl;

  constructor(baseUrl) { this.#baseUrl = baseUrl; }

  iniciarSesion(token) { this.#token = token; }

  async get(ruta) {
    const r = await fetch(this.#baseUrl + ruta, {
      headers: this.#token ? { Authorization: `Bearer ${this.#token}` } : {},
    });
    if (!r.ok) throw new Error(`HTTP ${r.status}`);
    return r.json();
  }
}
```

Nadie puede hacer `cliente.#token` ni lo verá al imprimir el objeto con `JSON.stringify`. Aun así, recuerda que en el navegador cualquier dato es accesible para quien controla DevTools: la privacidad de `#` protege de errores de programación, no de un atacante con acceso a la página.

### Cuándo usarlo y cuándo no
Úsalo para estado interno que no debe cambiarse desde fuera y para métodos auxiliares. No lo uses si necesitas que las subclases accedan al campo (JS no tiene `protected`; usa `_campo` por convención o métodos). Ten en cuenta que los `Proxy` no funcionan bien con objetos que usan privados: el proxy no tiene los campos y el acceso falla con `TypeError`.

### Errores comunes
- **Creer que `_campo` es privado**.
- **Usar `this.#x` sin declararlo** en el cuerpo de la clase: error de sintaxis.
- **Esperar que una subclase vea `#x`** del padre.
- **Envolver la instancia en un `Proxy`**: los métodos que tocan privados lanzan `TypeError`.

### Para profundizar
- [Propiedades privadas (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_properties)
- [Clases (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)
- [Propuesta de campos de clase (TC39)](https://github.com/tc39/proposal-class-fields)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace el prefijo `#` en un campo de clase?
> Lo vuelve privado de verdad: solo es accesible dentro del cuerpo de la clase. Fuera, `obj.#x` es un **error de sintaxis**, no `undefined`.

> [!question]- ¿Qué diferencia hay con la convención `_campo`?
> `_campo` es solo una convención: cualquiera puede leerlo y escribirlo. `#campo` lo garantiza el lenguaje.

> [!question]- ¿Cómo compruebas si un objeto tiene un campo privado de tu clase?
> Con `#campo in obj` dentro de la clase (devuelve booleano sin lanzar error).

