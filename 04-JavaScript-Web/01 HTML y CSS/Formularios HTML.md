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
# Formularios HTML

Parte de: [[04-JavaScript-Web/01 HTML y CSS/_Índice HTML y CSS|HTML y CSS]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un formulario HTML (`<form>`) agrupa controles (`<input>`, `<select>`, `<textarea>`, `<button>`) para que el usuario capture datos y el navegador los **envíe** a un servidor o los entregue a tu JavaScript. Bien construido, te da gratis accesibilidad, validación básica, autocompletado, teclados adecuados en el celular y envío con la tecla Enter.

### Cómo funciona
**Etiquetas.** Cada control necesita un `<label>` asociado, con `for` igual al `id` del control (o envolviendo el control). Esa asociación:
- da al campo su **nombre accesible** (el lector de pantalla anuncia "Correo, campo de edición"),
- hace que **clicar la etiqueta enfoque o marque** el control (área de clic más grande, clave en casillas y radios en móvil),
- y permite que las pruebas automatizadas encuentren el campo por su texto.

El `placeholder` no sustituye la etiqueta: desaparece al escribir y suele tener poco contraste.

**Validación nativa** (sin JS):
- `required`: obligatorio.
- `type="email"`, `type="url"`, `type="number"`, `type="date"`: validan el formato y en móvil muestran el teclado correspondiente.
- `minlength` / `maxlength`, `min` / `max` / `step`.
- `pattern="[A-Z]{4}[0-9]{6}..."`: expresión regular que debe cumplir todo el valor.

El navegador bloquea el envío y muestra un mensaje si algo falla. Las pseudoclases `:invalid`, `:valid` y `:user-invalid` (esta solo después de que el usuario interactuó) permiten darles estilo. Desde JS, `form.checkValidity()`, `input.validity` y `input.setCustomValidity('mensaje')` extienden esa validación. Ojo: la validación del cliente es comodidad; **el servidor siempre valida** (en tu API .NET, con FluentValidation).

**Envío.** Al enviar, el navegador recorre los controles **con atributo `name`** y arma pares `name=valor`. `method="get"` los pone en la URL; `method="post"` en el cuerpo. Con archivos se requiere `enctype="multipart/form-data"`. Desde JS normalmente interceptas el evento `submit` y usas `FormData` (ver [[Formularios y FormData]]).

**Qué NO se envía:**
- controles **sin `name`**,
- controles **`disabled`** (o dentro de un `<fieldset disabled>`),
- **casillas y radios sin marcar** (no mandan nada, ni siquiera "false"),
- botones distintos del que disparó el envío,
- elementos fuera del `<form>` que no lo referencien con `form="id"`.

Los campos `readonly` **sí** se envían; `disabled` no. Es una diferencia frecuente en entrevistas.

**Botones.** Un `<button>` dentro de un `<form>` sin `type` es `type="submit"` por defecto: al clicarlo envía el formulario y recarga la página. Para botones que solo ejecutan JS (mostrar contraseña, agregar renglón) usa `type="button"`. `type="reset"` regresa los valores iniciales (rara vez deseable).

### Ejemplo
Registro de cliente para Itzamna.Clio:

```html
<form id="alta-cliente" action="/api/clientes" method="post" novalidate>
  <fieldset>
    <legend>Datos del cliente</legend>

    <label for="nombre">Nombre</label>
    <input id="nombre" name="nombre" required autocomplete="name">

    <label for="correo">Correo</label>
    <input id="correo" name="correo" type="email" required autocomplete="email"
           aria-describedby="correo-ayuda">
    <small id="correo-ayuda">Te enviaremos la factura aquí.</small>

    <label for="rfc">RFC</label>
    <input id="rfc" name="rfc" required minlength="12" maxlength="13"
           pattern="[A-ZÑ&]{3,4}[0-9]{6}[A-Z0-9]{3}">

    <label><input type="checkbox" name="boletin" value="si"> Recibir boletín</label>
  </fieldset>

  <button type="button" id="limpiar">Limpiar</button>
  <button type="submit">Guardar</button>
</form>
```

```js
const form = document.getElementById('alta-cliente');
form.addEventListener('submit', async (e) => {
  e.preventDefault();
  if (!form.checkValidity()) { form.reportValidity(); return; }
  const datos = Object.fromEntries(new FormData(form)); // boletin no aparece si no se marcó
  const r = await fetch(form.action, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(datos),
  });
  // si r.status es 400, muestra los errores del ProblemDetails junto a cada campo
});
```

`novalidate` desactiva los globos nativos para mostrar mensajes propios, pero mantiene la API de validación disponible. `autocomplete` ayuda a los gestores de contraseñas y al autollenado.

### Cuándo usarlo y cuándo no
Usa `<form>` siempre que haya captura de datos, aunque envíes con `fetch`: obtienes Enter para enviar, validación y semántica. Agrupa campos relacionados con `<fieldset>` y `<legend>` (obligatorio para grupos de radios). Para controles muy especiales (selector de fechas con rangos, autocompletado) evalúa componentes accesibles antes de construirlos. Los modales con formulario combinan bien con [[Elemento dialog]] y `method="dialog"`.

### Errores comunes
- **`<button>` sin `type`** que recarga la página inesperadamente.
- **Campos sin `name`**: "el servidor no recibe el valor".
- **`disabled` para "no editable"**: no se envía; usa `readonly`.
- **Esperar `false` de una casilla sin marcar**: no llega nada; el backend debe tratar la ausencia como falso.
- **Confiar solo en la validación del cliente**: cualquiera puede saltarla con DevTools o curl.

### Para profundizar
- [Formularios web (MDN)](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Forms)
- [Validación de formularios en el cliente (MDN)](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Forms/Form_validation)
- [`<button>` y su atributo `type` (MDN)](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button)
- [Formularios en la especificación HTML (WHATWG)](https://html.spec.whatwg.org/multipage/forms.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Para qué sirve asociar `<label for="id">` con su `<input id="id">`?
> El lector de pantalla lee la etiqueta al enfocar el campo y hacer clic en la etiqueta enfoca el campo (área de clic más grande). Alternativa: envolver el input dentro del `<label>`.

> [!question]- ¿Qué validaciones nativas tienes sin escribir JS?
> Atributos `required`, `type="email"`/`url`/`number`, `min`/`max`, `minlength`/`maxlength`, `pattern`. El navegador bloquea el envío y expone el estado en `input.validity` y en las pseudoclases `:invalid`/`:valid` (o `:user-invalid`, que solo marca tras interactuar).

> [!question]- ¿Qué pasa con un `<button>` dentro de un `<form>` si no le pones `type`?
> Su tipo por defecto es `submit`: al hacer clic envía el formulario (y recarga la página si no haces `preventDefault`). Para botones que no envían, usa `type="button"`.

> [!question]- ¿Qué campos NO se envían en el formulario?
> Los que no tienen atributo `name`, los `disabled`, las casillas/radios sin marcar y los botones que no fueron el que disparó el envío. Si quieres un campo de solo lectura que sí se envíe, usa `readonly` en vez de `disabled`.

## Práctica

- [[Lab 1 - Página de registro accesible y responsiva]]

