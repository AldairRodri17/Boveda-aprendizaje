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
# Roles y atributos ARIA

Parte de: [[Accesibilidad web]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**WAI-ARIA** (*Accessible Rich Internet Applications*) es una especificación del W3C que agrega atributos al HTML para describir a las tecnologías de asistencia lo que el HTML nativo no puede expresar: el **rol** de un componente personalizado (`role="tab"`), su **estado** (`aria-expanded="true"`) y sus **relaciones** (qué texto lo etiqueta o describe). ARIA **solo cambia lo que se anuncia** en el árbol de accesibilidad; no agrega comportamiento: un `div role="button"` no se vuelve enfocable ni responde a Enter por sí solo.

La **primera regla de ARIA**: si existe un elemento HTML nativo con la semántica que necesitas, úsalo. ARIA es para los huecos, no para reemplazar `<button>`, `<nav>` o `<label>`.

### Cómo funciona
Cada elemento en el árbol de accesibilidad tiene rol, nombre, descripción y estados. ARIA permite modificar cada parte:

**Nombre y descripción**
- `aria-label="Cerrar"`: da el nombre accesible con un texto directo. Útil en botones de solo ícono.
- `aria-labelledby="id1 id2"`: toma el nombre del texto de otro(s) elemento(s) visibles. Tiene prioridad sobre `aria-label` y sobre el contenido. Ideal para que un diálogo se llame como su título.
- `aria-describedby="id"`: agrega una **descripción** adicional que se lee después del nombre: instrucciones o mensajes de error de un input.

**Estados comunes**: `aria-expanded` (menú abierto/cerrado), `aria-pressed` (botón alternable), `aria-selected` (pestaña activa), `aria-invalid` (campo con error), `aria-current="page"` (enlace de la página actual), `aria-disabled`.

**Regiones vivas**: cuando un texto cambia sin que el foco se mueva (un toast de "Guardado", un contador de resultados), el lector no se entera. Con `aria-live` lo anuncia:
- `aria-live="polite"`: anuncia el cambio cuando el lector termine lo que está diciendo. Es lo correcto para casi todo.
- `aria-live="assertive"`: interrumpe; resérvalo para errores urgentes.
- `role="status"` equivale a una región *polite*; `role="alert"` a una *assertive*.

La región debe existir en el DOM **antes** de cambiar su texto; si la creas junto con el mensaje, muchos lectores no la anuncian.

**Ocultar**: `aria-hidden="true"` quita el elemento y sus hijos del árbol de accesibilidad, pero **sigue visible y enfocable**. Si contiene un botón o enlace, el usuario de teclado llega a un elemento que el lector no anuncia: un "foco fantasma". Para ocultar del todo usa `hidden`/`display:none`; para desactivar una zona completa (el fondo de un modal), el atributo `inert`.

### Ejemplo
Un acordeón y un aviso de guardado en Itzamna.Clio:

```html
<h3>
  <button type="button" aria-expanded="false" aria-controls="det-1">
    Datos fiscales
  </button>
</h3>
<div id="det-1" hidden>...</div>

<div id="aviso" role="status"></div>
```

```js
const btn = document.querySelector('[aria-controls="det-1"]');
const panel = document.getElementById('det-1');
btn.addEventListener('click', () => {
  const abierto = btn.getAttribute('aria-expanded') === 'true';
  btn.setAttribute('aria-expanded', String(!abierto));
  panel.hidden = abierto;
});

// La región ya existe; solo cambiamos su texto
async function guardar() {
  // ... llamada a la API
  document.getElementById('aviso').textContent = 'Cambios guardados';
}
```

Usa `<button>` (nativo) dentro del encabezado: ya es enfocable y responde a Enter/Espacio; ARIA solo agrega el estado `aria-expanded`.

### Cuándo usarlo y cuándo no
Úsalo para widgets que HTML no tiene (pestañas, combobox con autocompletado, árboles, menús de aplicación) siguiendo los patrones de la APG, que indican qué roles, estados y teclas implementar. No lo uses para "decorar" elementos nativos (`<button role="button">` es redundante) ni para sustituir texto visible que podría ser una `<label>`. Ver también [[Navegación con teclado y foco]].

### Errores comunes
- **`aria-hidden="true"` en algo enfocable** o en un contenedor con controles: foco fantasma.
- **Rol sin comportamiento**: `div role="button"` sin `tabindex="0"` ni manejo de Enter/Espacio.
- **`aria-label` que contradice el texto visible**: los usuarios de control por voz dicen lo que ven y no funciona. El nombre debe incluir el texto visible.
- **Región viva creada al vuelo**: créala vacía al cargar la página.
- **Roles de pestañas sin la navegación con flechas** que esos roles prometen.

### Para profundizar
- [WAI-ARIA 1.2 (W3C)](https://www.w3.org/TR/wai-aria-1.2/)
- [ARIA Authoring Practices Guide](https://www.w3.org/WAI/ARIA/apg/)
- [ARIA en MDN](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Para qué sirven `aria-label`, `aria-labelledby` y `aria-describedby`?
> Dan nombre accesible a un elemento (`aria-label` con texto directo; `aria-labelledby` apuntando al id de otro elemento) o una descripción adicional (`aria-describedby`, p. ej. el mensaje de error de un campo).

> [!question]- ¿Qué hace `aria-live="polite"`?
> Marca una región cuyo contenido, al cambiar, se anuncia en el lector de pantalla cuando el usuario termina lo que hace. Útil para toasts o "Guardado". `assertive` interrumpe; úsalo poco.

> [!question]- Error común: `aria-hidden="true"` en un elemento con foco. ¿Por qué es un problema?
> El elemento sigue recibiendo foco con Tab pero el lector no lo anuncia: el usuario cae en un "hueco" silencioso. Si lo ocultas, quítale también el foco (`inert` o `tabindex="-1"` en sus controles).

