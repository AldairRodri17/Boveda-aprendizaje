---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: JavaScript y Web
tags: [javascript-web]
---
# CSS - diseño responsivo

Parte de: [[04-JavaScript-Web/01 HTML y CSS/_Índice HTML y CSS|HTML y CSS]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El diseño responsivo hace que una misma página se adapte a cualquier tamaño de pantalla (tu iPhone 15 Pro, una tablet, un monitor ancho) sin versiones separadas. Se basa en tres ideas: **layouts fluidos** (tamaños relativos en vez de píxeles fijos), **imágenes flexibles** y **reglas condicionales** (media queries y container queries) que ajustan el diseño en ciertos puntos.

### Cómo funciona
**1. La etiqueta viewport.** Sin ella, los navegadores móviles simulan una pantalla de escritorio (unos 980 px) y luego reducen todo, así que tus media queries no se disparan y el texto se ve diminuto.

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

`width=device-width` iguala el ancho del viewport al ancho del dispositivo en píxeles CSS; `initial-scale=1` arranca sin zoom.

**2. Media queries.** Aplican reglas según características del **viewport**: ancho, orientación, preferencias del usuario (`prefers-color-scheme`, `prefers-reduced-motion`).

**Mobile first** significa escribir primero los estilos para la pantalla pequeña (sin media query) y agregar cambios para pantallas mayores con `min-width`. Así el móvil carga el CSS más simple y el diseño crece por capas.

```css
.listado { display: grid; gap: 1rem; }                 /* móvil: 1 columna */
@media (min-width: 48rem) { .listado { grid-template-columns: 1fr 1fr; } }
@media (min-width: 72rem) { .listado { grid-template-columns: repeat(3, 1fr); } }
```

Los *breakpoints* deben salir del contenido ("aquí la tabla ya no cabe"), no de modelos de teléfono.

**3. Container queries.** Una media query pregunta por la pantalla; una **container query** pregunta por el tamaño del **contenedor** del componente. Así una tarjeta se acomoda igual si está en una columna angosta lateral o en el área principal, sin saber dónde la pusieron. Es ideal para componentes reutilizables (React, Web Components).

```css
.zona-tarjetas { container-type: inline-size; }
.tarjeta { display: grid; gap: .5rem; }
@container (min-width: 30rem) {
  .tarjeta { grid-template-columns: 120px 1fr; } /* imagen al lado */
}
```

**4. Tipografía y espacios fluidos con `clamp()`.** `clamp(mínimo, preferido, máximo)` devuelve el valor preferido pero nunca por debajo del mínimo ni por encima del máximo. Combinado con unidades de viewport, logra tamaños que crecen suavemente sin media queries:

```css
h1 { font-size: clamp(1.5rem, 1rem + 2.5vw, 3rem); }
.contenedor { padding-inline: clamp(1rem, 5vw, 4rem); }
```

Incluir un término en `rem` (como `1rem + 2.5vw`) mantiene el texto ajustable con el zoom del usuario.

**5. Unidades relativas.** `rem` (relativa a la fuente raíz) para tipografía y espacios; `%`, `fr` y `vw/vh` para layout; `ch` para limitar el ancho de líneas de texto (`max-width: 65ch`). Para la altura de pantalla en móvil, `dvh` considera las barras del navegador que aparecen y desaparecen.

### Ejemplo
Panel de Itzamna.Clio que en móvil apila y en escritorio pone menú lateral:

```css
.app { display: grid; grid-template-areas: "header" "main"; min-height: 100dvh; }
.app > nav { display: none; }

@media (min-width: 64rem) {
  .app { grid-template-columns: 16rem 1fr; grid-template-areas: "header header" "nav main"; }
  .app > nav { display: block; grid-area: nav; }
}
img { max-width: 100%; height: auto; } /* imágenes que nunca desbordan */
```

Para servir imágenes de distinto tamaño según la pantalla, ver [[Imágenes responsive]]. Prueba con el modo dispositivo de DevTools (Ctrl+Shift+M) y en un teléfono real: el emulador no reproduce el teclado virtual ni la barra de direcciones.

### Cuándo usarlo y cuándo no
Siempre en sitios públicos y en sistemas internos que se usen desde el celular. Usa media queries para el **layout de la página** y para preferencias del usuario; container queries para **componentes**. Muchas veces no necesitas ninguna de las dos: `flex-wrap`, `grid` con `auto-fit` y `clamp()` resuelven buena parte del trabajo sin breakpoints (ver [[CSS - grid]] y [[CSS - flexbox]]).

### Errores comunes
- **Olvidar la etiqueta viewport**: las media queries "no funcionan" en el celular.
- **`user-scalable=no` o `maximum-scale=1`**: impide el zoom y es una falla de accesibilidad.
- **Breakpoints por dispositivo** (iPhone, iPad): cambian cada año. Usa los del contenido.
- **Anchos fijos en px** en contenedores: generan scroll horizontal.
- **`100vh` en móvil**: puede quedar tapado por la barra del navegador; usa `100dvh` o `100svh`.

### Para profundizar
- [Diseño responsivo (MDN)](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Responsive_Design)
- [Container queries (MDN)](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_containment/Container_queries)
- [`clamp()` (MDN)](https://developer.mozilla.org/en-US/docs/Web/CSS/clamp)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es *mobile first* en media queries?
> Escribir los estilos base para pantallas pequeñas y añadir `@media (min-width: …)` para ampliar. Suele producir CSS más simple que ir quitando cosas con `max-width`.

> [!question]- ¿Para qué sirve `<meta name="viewport" content="width=device-width, initial-scale=1">`?
> Sin ella, el navegador móvil simula una pantalla de escritorio (~980px) y escala todo hacia abajo; tus media queries no funcionan como esperas.

> [!question]- ¿Qué ventaja tienen las *container queries* (`@container`) sobre las media queries?
> Responden al tamaño del **contenedor** y no al de la ventana, así un componente (tarjeta) se adapta igual en una columna angosta o en el ancho completo.

> [!question]- ¿Para qué usarías `clamp()`?
> Para valores fluidos con límites: `font-size: clamp(1rem, 2.5vw, 2rem)` crece con la ventana pero nunca baja de 1rem ni pasa de 2rem.

## Práctica

- [[Lab 1 - Página de registro accesible y responsiva]]

