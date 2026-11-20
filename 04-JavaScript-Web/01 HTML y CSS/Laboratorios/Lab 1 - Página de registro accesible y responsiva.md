---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: JavaScript y Web
herramienta: VS Code, navegador y Lighthouse (DevTools)
tags: [javascript-web, laboratorio]
---
# Lab 1 - Página de registro accesible y responsiva

Parte de: [[04-JavaScript-Web/01 HTML y CSS/_Índice HTML y CSS|HTML y CSS]]

**Cubre:** [[HTML semántico]] · [[Formularios HTML]] · [[Accesibilidad web]] · [[Navegación con teclado y foco]] · [[Contraste y texto alternativo]] · [[CSS - flexbox]] · [[CSS - grid]] · [[CSS - diseño responsivo]] · [[CSS - variables]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Construir una página con encabezado, navegación, formulario de registro y pie, que funcione solo con teclado, pase la auditoría de accesibilidad de Lighthouse y cambie de diseño entre móvil y escritorio, con tema claro/oscuro.

## Pasos

1. Crea `registro.html` y `estilos.css` en esta carpeta. Abre con la extensión Live Server de VS Code (o cualquier servidor estático).
2. Estructura con `<header>`, `<nav>`, `<main>` y `<footer>`; un solo `<h1>`.
3. Formulario con nombre, correo, contraseña (mínimo 8), fecha de nacimiento, casilla de términos y botón de enviar. Cada campo con `<label>` asociado y validación nativa.
4. Agrega un `<p>` de ayuda bajo la contraseña y asócialo con `aria-describedby`.
5. Layout: en móvil todo en una columna; desde 768px, `<main>` con grid de dos columnas (texto informativo + formulario). La barra de navegación con flexbox.
6. Define colores en variables CSS en `:root` y un tema oscuro con `@media (prefers-color-scheme: dark)`. Agrega un estilo visible con `:focus-visible`.
7. Recorre la página solo con Tab/Shift+Tab/Enter. Corre Lighthouse (categoría Accessibility) en modo móvil y escritorio.
8. Emula tema oscuro en DevTools (Rendering → *Emulate CSS media feature prefers-color-scheme*) y revisa contraste.

## Criterios de terminado

- [ ] Al hacer clic en cada etiqueta, se enfoca su campo.
- [ ] Enviar vacío muestra los mensajes nativos y no envía; un correo inválido se rechaza.
- [ ] Todo el formulario se completa y envía solo con teclado, y el foco siempre es visible.
- [ ] Lighthouse Accessibility ≥ 95 en móvil.
- [ ] A menos de 768px hay una columna; a partir de 768px, dos.
- [ ] Con tema oscuro emulado los colores cambian y no hay avisos de contraste en Lighthouse.

## Solución

> [!success]- HTML del formulario
> ```html
> <form id="registro">
>   <label for="nombre">Nombre</label>
>   <input id="nombre" name="nombre" required autocomplete="name">
>
>   <label for="correo">Correo</label>
>   <input id="correo" name="correo" type="email" required autocomplete="email">
>
>   <label for="pass">Contraseña</label>
>   <input id="pass" name="pass" type="password" minlength="8" required aria-describedby="pass-ayuda">
>   <p id="pass-ayuda">Mínimo 8 caracteres.</p>
>
>   <label for="nac">Fecha de nacimiento</label>
>   <input id="nac" name="nac" type="date">
>
>   <label><input type="checkbox" name="terminos" required> Acepto los términos</label>
>   <button type="submit">Crear cuenta</button>
> </form>
> ```

> [!success]- CSS clave
> ```css
> :root { --fondo: #fff; --texto: #1a1a1a; --acento: #0b57d0; color-scheme: light dark; }
> @media (prefers-color-scheme: dark) {
>   :root { --fondo: #121212; --texto: #ececec; --acento: #8ab4f8; }
> }
> body { background: var(--fondo); color: var(--texto); }
> nav ul { display: flex; gap: 1rem; list-style: none; flex-wrap: wrap; }
> main { display: grid; gap: 2rem; }
> @media (min-width: 768px) { main { grid-template-columns: 1fr 1fr; } }
> form { display: grid; gap: .5rem; }
> :focus-visible { outline: 3px solid var(--acento); outline-offset: 2px; }
> ```

## Notas personales

_Qué me costó, errores que cometí, dudas._
