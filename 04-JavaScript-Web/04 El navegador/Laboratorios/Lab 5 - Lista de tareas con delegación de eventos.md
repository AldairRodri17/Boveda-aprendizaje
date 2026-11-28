---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: JavaScript y Web
herramienta: VS Code y navegador
tags: [javascript-web, laboratorio]
contenido: completo
---
# Lab 5 - Lista de tareas con delegación de eventos

Parte de: [[04-JavaScript-Web/04 El navegador/_Índice El navegador|El navegador]]

**Cubre:** [[DOM - selección y creación]] · [[DOM - eventos]] · [[Propagación de eventos (bubbling y capturing)]] · [[Delegación de eventos]] · [[Formularios y FormData]] · [[localStorage]] · [[Seguridad del DOM (innerHTML y escape de datos)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Hacer una lista de tareas en JS plano con **un solo listener** de clic para toda la lista, que agregue, marque y borre tareas, persista en `localStorage` y sea segura ante texto malicioso.

## Pasos

1. Crea `tareas.html` y `tareas.js` (módulo). Un `<form>` con un input y una `<ul id="lista">`.
2. Al enviar el form, lee el texto con `FormData`, agrega la tarea al estado y repinta.
3. Pinta cada tarea con `createElement`/`textContent` (nada de `innerHTML` con el texto del usuario). Cada `li` tiene casilla y un botón con ícono dentro (`<button data-accion="borrar"><span>🗑</span></button>`).
4. Registra **un** listener `click` en la `ul` que use `closest('[data-accion]')` para borrar, y un listener `change` para marcar.
5. Guarda y carga el estado en `localStorage` con `try/catch` en el `JSON.parse`.
6. Agrega temporalmente un listener en `document` con `{ capture: true }` que haga `console.log('captura')`, y otro normal en la `ul` que haga `console.log('burbujeo')`; observa el orden.
7. Prueba agregar la tarea `<img src=x onerror=alert(1)>`.

## Criterios de terminado

- [ ] Hay exactamente un listener de clic en la lista (compruébalo en DevTools → Elements → Event Listeners).
- [ ] Hacer clic sobre el ícono dentro del botón también borra la tarea.
- [ ] Las tareas agregadas después de cargar funcionan sin registrar listeners nuevos.
- [ ] Al recargar, las tareas siguen ahí.
- [ ] La tarea `<img src=x onerror=alert(1)>` se muestra como texto y no aparece ninguna alerta.
- [ ] En la consola, `captura` sale antes que `burbujeo`.

## Solución

> [!success]- Código clave
> ```js
> const lista = document.querySelector('#lista');
> let tareas = cargar();
>
> function cargar() {
>   try { return JSON.parse(localStorage.getItem('tareas')) ?? []; } catch { return []; }
> }
> function guardar() { localStorage.setItem('tareas', JSON.stringify(tareas)); }
>
> function pintar() {
>   lista.replaceChildren(...tareas.map(t => {
>     const li = document.createElement('li');
>     li.dataset.id = t.id;
>     const chk = Object.assign(document.createElement('input'), { type: 'checkbox', checked: t.hecha });
>     const txt = document.createElement('span');
>     txt.textContent = t.texto;                    // seguro
>     const btn = document.createElement('button');
>     btn.dataset.accion = 'borrar';
>     btn.setAttribute('aria-label', `Borrar ${t.texto}`);
>     btn.innerHTML = '<span aria-hidden="true">🗑</span>'; // HTML fijo, no del usuario
>     li.append(chk, txt, btn);
>     return li;
>   }));
> }
>
> lista.addEventListener('click', (e) => {
>   const btn = e.target.closest('[data-accion="borrar"]');
>   if (!btn || !lista.contains(btn)) return;
>   const id = btn.closest('li').dataset.id;
>   tareas = tareas.filter(t => t.id !== id);
>   guardar(); pintar();
> });
> lista.addEventListener('change', (e) => {
>   const id = e.target.closest('li')?.dataset.id;
>   tareas = tareas.map(t => t.id === id ? { ...t, hecha: e.target.checked } : t);
>   guardar();
> });
> document.querySelector('form').addEventListener('submit', (e) => {
>   e.preventDefault();
>   const texto = new FormData(e.currentTarget).get('texto').trim();
>   if (!texto) return;
>   tareas.push({ id: crypto.randomUUID(), texto, hecha: false });
>   e.currentTarget.reset(); guardar(); pintar();
> });
> pintar();
> ```

## Notas personales

_Qué me costó, errores que cometí, dudas._
