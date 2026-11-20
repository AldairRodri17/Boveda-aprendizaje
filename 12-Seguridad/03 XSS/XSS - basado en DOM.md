---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Seguridad
tags: [seguridad]
---
# XSS - basado en DOM

Parte de: [[12-Seguridad/03 XSS/_Índice XSS|XSS]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El **XSS basado en DOM** ocurre **enteramente en el navegador**: el JavaScript del cliente toma un dato de una **fuente** que el usuario controla (la URL, el fragmento `#`, `localStorage`, un mensaje `postMessage`) y lo pasa a un **sumidero** peligroso (una API que interpreta HTML o ejecuta código) sin tratarlo. El servidor puede estar perfecto; el problema está en el código del front.

### Cómo funciona
Se piensa en términos de **fuentes** (de dónde sale el dato no confiable) y **sumideros** (dónde, al escribirlo, se convierte en código):

```mermaid
flowchart LR
  F["Fuente: location.hash,<br/>location.search, postMessage"] --> JS[JavaScript del cliente]
  JS --> Sink{"Sumidero peligroso"}
  Sink -->|innerHTML, outerHTML| H[Se interpreta como HTML]
  Sink -->|eval, Function, setTimeout(string)| E[Se ejecuta como código]
  Sink -->|location, href| R[Redirección o javascript:]
```

Fuentes típicas: `location` (`search`, `hash`, `href`), `document.referrer`, `window.name`, datos de `postMessage`, valores leídos de `localStorage`/`sessionStorage`. Sumideros típicos: `element.innerHTML`/`outerHTML`, `insertAdjacentHTML`, `document.write`, `eval`, `Function`, `setTimeout`/`setInterval` con una cadena, asignar a `location`/`href`, y `jQuery.html()`.

**Por qué no lo detectan los filtros del servidor**: el dato peligroso puede no llegar nunca al servidor. El fragmento de la URL después de `#` **no se envía** en la petición HTTP, así que un WAF o una validación en el back no lo ven. Y aunque el dato viaje al servidor, el problema está en cómo lo usa el JavaScript del cliente, no en la respuesta del servidor. Por eso se detecta revisando el **código del front** y con herramientas que analizan el flujo en el navegador.

### Ejemplo
Patrón vulnerable y su corrección en JavaScript del cliente:

```js
// Vulnerable: dato del hash a un sumidero de HTML
const seccion = decodeURIComponent(location.hash.slice(1));
document.querySelector("#titulo").innerHTML = seccion;   // sumidero peligroso

// Seguro: tratar el dato como texto
document.querySelector("#titulo").textContent = seccion;
```

En **React** el XSS de DOM reaparece sobre todo con `dangerouslySetInnerHTML` o al construir URLs con datos del usuario:

```tsx
// Riesgo: un href tomado del parámetro puede ser javascript:
const destino = new URLSearchParams(location.search).get("volver") ?? "/";

// Seguro: validar que sea una ruta interna antes de usarla
const esRutaInterna = /^\/[A-Za-z0-9/_-]*$/.test(destino);
<a href={esRutaInterna ? destino : "/"}>Volver</a>
```

**Cómo lo previenes**:
1. **Evita los sumideros peligrosos**: usa `textContent` en lugar de `innerHTML`, crea nodos con `document.createElement` y propiedades, y nunca `eval`/`Function` con datos.
2. Si necesitas HTML, **sanitiza** con DOMPurify antes de asignarlo. Ver [[XSS - sanitización de HTML (DOMPurify)]].
3. **Valida las URLs** (solo rutas internas o esquemas `https:`) antes de asignarlas a `href`/`location`.
4. Activa **[[Trusted Types]]** vía CSP: obliga a que los sumideros de DOM solo acepten valores marcados como seguros, y convierte estos errores en algo que el navegador bloquea.
5. Para `postMessage`, **valida el origen**. Ver [[postMessage seguro (validar origen)]].

### Cuándo usarlo y cuándo no
La prevención aplica a cualquier código de cliente que lea fuentes controlables y escriba en el DOM o navegue. Es especialmente relevante en SPAs (Itzamna.Clio) que leen parámetros de la URL para decidir qué mostrar. Para practicar de forma legal, PortSwigger tiene laboratorios de DOM XSS.

### Errores comunes
- **Asumir que el servidor "ya filtró"**: el hash ni siquiera llega al servidor.
- **Usar `innerHTML`** por costumbre cuando `textContent` bastaba.
- **`eval` o `new Function`** sobre datos "de confianza".
- **Asignar a `href`/`location`** un valor del usuario sin validar el esquema (`javascript:`).
- **Olvidar `postMessage`** como fuente no confiable.

### Para profundizar
- [PortSwigger: DOM-based XSS](https://portswigger.net/web-security/cross-site-scripting/dom-based)
- [OWASP DOM based XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html)
- [MDN: Trusted Types](https://developer.mozilla.org/en-US/docs/Web/API/Trusted_Types_API)

> [!warning] Ética
> Las pruebas de esta nota se hacen solo en tu propio código, en tu máquina o en plataformas de entrenamiento (PortSwigger Web Security Academy, DVWA, OWASP Juice Shop). Ver [[Marco legal y autorización por escrito]].

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es el XSS basado en DOM?
> El servidor no interviene: un script del cliente lee una fuente controlable (`location.hash`, `location.search`, `postMessage`, `localStorage`) y la pasa a un sink peligroso (`innerHTML`, `document.write`, `eval`, `setTimeout` con string, atributos `href`).

> [!question]- ¿Por qué no lo detectan los filtros del servidor?
> Porque el dato peligroso puede no viajar al servidor (el fragmento `#` no se envía) y todo ocurre en el navegador.

> [!question]- ¿Cómo lo previenes?
> Usar sinks seguros (`textContent`, `setAttribute` en atributos no ejecutables, `createElement`), validar URLs antes de asignarlas a `href`, y activar [[Trusted Types]] para que el navegador rechace asignaciones peligrosas.

## Práctica

- [[Lab 6 - PortSwigger - laboratorios de XSS nivel Apprentice]]

