---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: JavaScript y Web
herramienta: VS Code y navegador
tags: [javascript-web, laboratorio]
---
# Lab 6 - Web Component con Shadow DOM

Parte de: [[04-JavaScript-Web/04 El navegador/_Índice El navegador|El navegador]]

**Cubre:** [[Web Components]] · [[Shadow DOM]] · [[CSS - variables]] · [[DOM - eventos]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Crear un componente `<contador-chip>` reutilizable con Shadow DOM, que acepte atributos, exponga estilos por variables CSS y `::part`, use un slot y emita un evento personalizado.

## Pasos

1. Crea `contador-chip.js` y `demo.html` en esta carpeta.
2. Define la clase con `attachShadow({ mode: 'open' })`, un `<slot>` para la etiqueta y botones − y +.
3. Atributos `valor`, `min` y `max` en `observedAttributes`; al cambiar, repinta el número.
4. Estilos internos con `:host`, color de acento con `var(--chip-acento, #0b57d0)` y el número con `part="numero"`.
5. Al cambiar el valor, emite `cambio` con `detail: { valor }`, `bubbles: true` y `composed: true`.
6. En `demo.html` pon dos chips; personaliza uno con `--chip-acento` y `contador-chip::part(numero)`. Escucha `cambio` en `document`.
7. Comprueba que una regla global `button { background: red }` en la página NO afecta los botones internos.

## Criterios de terminado

- [ ] `document.querySelector('contador-chip').shadowRoot` existe y contiene los botones.
- [ ] Cambiar el atributo en DevTools (`valor="5"`) actualiza el número.
- [ ] El valor no baja de `min` ni sube de `max`.
- [ ] El listener en `document` recibe `cambio` con el valor correcto.
- [ ] La regla global de botones no cambia el componente, pero `--chip-acento` y `::part(numero)` sí.

## Solución

> [!success]- contador-chip.js
> ```js
> class ContadorChip extends HTMLElement {
>   static observedAttributes = ['valor', 'min', 'max'];
>   #root = this.attachShadow({ mode: 'open' });
>   constructor() {
>     super();
>     this.#root.innerHTML = `
>       <style>
>         :host { display: inline-flex; gap: .5rem; align-items: center; font: inherit; }
>         button { border: 0; border-radius: 999px; padding: .25rem .6rem; background: var(--chip-acento, #0b57d0); color: #fff; }
>       </style>
>       <slot></slot>
>       <button data-d="-1" aria-label="Restar">−</button>
>       <span part="numero"></span>
>       <button data-d="1" aria-label="Sumar">+</button>`;
>     this.#root.addEventListener('click', (e) => {
>       const d = Number(e.target.closest('button')?.dataset.d);
>       if (d) this.valor = this.valor + d;
>     });
>   }
>   get valor() { return Number(this.getAttribute('valor') ?? 0); }
>   set valor(v) {
>     const min = Number(this.getAttribute('min') ?? -Infinity), max = Number(this.getAttribute('max') ?? Infinity);
>     const n = Math.min(max, Math.max(min, v));
>     if (n === this.valor) return;
>     this.setAttribute('valor', n);
>     this.dispatchEvent(new CustomEvent('cambio', { detail: { valor: n }, bubbles: true, composed: true }));
>   }
>   attributeChangedCallback() { this.#root.querySelector('[part=numero]').textContent = this.valor; }
>   connectedCallback() { this.attributeChangedCallback(); }
> }
> customElements.define('contador-chip', ContadorChip);
> ```
> Uso: `<contador-chip valor="2" min="0" max="10">Tazas</contador-chip>`.

## Notas personales

_Qué me costó, errores que cometí, dudas._
