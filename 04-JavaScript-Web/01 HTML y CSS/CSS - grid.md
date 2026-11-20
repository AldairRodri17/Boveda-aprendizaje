---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: JavaScript y Web
tags: [javascript-web]
---
# CSS - grid

Parte de: [[04-JavaScript-Web/01 HTML y CSS/_Índice HTML y CSS|HTML y CSS]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**CSS Grid** es un sistema de layout de **dos dimensiones**: defines filas y columnas en el contenedor y colocas a los hijos en las celdas o áreas que quieras. Es la herramienta para la estructura de una página (header, menú lateral, contenido, footer), galerías de tarjetas y formularios alineados.

### Cómo funciona
Con `display: grid` el contenedor define **pistas** (*tracks*): columnas con `grid-template-columns` y filas con `grid-template-rows`. Entre pistas hay **líneas numeradas** desde 1. Los hijos se acomodan solos en orden (colocación automática) o los ubicas por líneas o por nombres de área.

Unidades y funciones clave:
- **`fr`**: una fracción del espacio libre. `1fr 2fr` reparte en proporción 1:2.
- **`repeat(n, pista)`**: evita repetir. `repeat(3, 1fr)`.
- **`minmax(min, max)`**: la pista mide al menos `min` y como máximo `max`.
- **`auto-fill` / `auto-fit`**: en lugar de un número, crea tantas columnas como quepan.

**Colocar por líneas**: `grid-column: 1 / 3` va de la línea 1 a la línea 3, es decir, ocupa **dos columnas**. Equivale a `grid-column: 1 / span 2`. `grid-column: 1 / -1` ocupa todo el ancho (la línea −1 es la última).

**Colocar por áreas**: `grid-template-areas` dibuja el layout con nombres, y cada hijo dice a qué área va con `grid-area`. Es muy legible y fácil de cambiar en una media query.

### Ejemplo: layout de aplicación
```css
.app {
  display: grid;
  grid-template-columns: 250px 1fr;
  grid-template-rows: auto 1fr auto;
  grid-template-areas:
    "header  header"
    "sidebar main"
    "footer  footer";
  min-height: 100dvh;
}
.app > header { grid-area: header; }
.app > aside  { grid-area: sidebar; }
.app > main   { grid-area: main; }
.app > footer { grid-area: footer; }

@media (max-width: 48rem) {
  .app {
    grid-template-columns: 1fr;
    grid-template-areas: "header" "main" "sidebar" "footer";
  }
}
```

La fila del medio con `1fr` estira el contenido para que el footer quede abajo aunque haya poco contenido.

### Galería sin media queries
```css
.tarjetas {
  display: grid;
  gap: 1rem;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
}
```

Esto significa: "crea tantas columnas como quepan, cada una de al menos 200 px; reparte el sobrante en partes iguales". En un contenedor de 900 px caben 4 (4·200 = 800, más los `gap`); al angostarse quedan 3, 2, 1. Es responsivo sin breakpoints.

**`auto-fill` vs `auto-fit`**: las dos crean el máximo de columnas que caben. La diferencia aparece cuando **hay menos ítems que columnas**:
- `auto-fill` conserva las columnas vacías: con 2 tarjetas en 900 px, quedan 2 tarjetas de ~225 px y 2 huecos.
- `auto-fit` colapsa las columnas vacías a 0 y los `1fr` se estiran: las 2 tarjetas ocupan todo el ancho.

### Centrar en una línea
```css
.centro { display: grid; place-items: center; }
```

`place-items` es el atajo de `align-items` + `justify-items`.

### Otros detalles útiles
- **`gap`** (`row-gap`, `column-gap`) separa sin márgenes.
- **Colocación implícita**: los ítems que no caben en las filas definidas crean filas nuevas con el tamaño de `grid-auto-rows`.
- **`subgrid`**: un hijo puede heredar las pistas del padre para alinear su contenido interno con la cuadrícula exterior (útil para que títulos y botones de tarjetas distintas queden a la misma altura).
- El inspector de grid de DevTools dibuja líneas, números y nombres de áreas.

### Cuándo usarlo y cuándo no
Usa grid cuando el **contenedor** manda la estructura en filas y columnas. Usa [[CSS - flexbox]] cuando el **contenido** manda y es una sola dirección (una barra de botones, un ícono con texto). No uses tablas HTML para maquetar; las tablas son para datos tabulares. Para adaptar componentes al espacio disponible, combina grid con container queries (ver [[CSS - diseño responsivo]]).

### Errores comunes
- **Confundir líneas con columnas**: `1 / 3` son dos columnas, no tres.
- **`grid-template-areas` mal formado**: cada fila debe tener el mismo número de celdas y cada área debe ser rectangular; si no, la regla completa se ignora.
- **`1fr` con contenido largo**: `1fr` equivale a `minmax(auto, 1fr)`, así que un contenido ancho puede ensanchar la columna. Usa `minmax(0, 1fr)`.
- **Reordenar visualmente** contenido con áreas sin cuidar el orden del DOM: afecta teclado y lectores de pantalla.

### Para profundizar
- [Conceptos básicos de grid (MDN)](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_grid_layout/Basic_concepts_of_grid_layout)
- [Áreas de plantilla con nombre (MDN)](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_grid_layout/Grid_template_areas)
- [CSS Grid Layout Module (W3C)](https://www.w3.org/TR/css-grid-2/)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Escribe el CSS de un layout con header arriba, sidebar de 250px a la izquierda, contenido a la derecha y footer abajo.
> ```css
> .layout {
>   display: grid;
>   grid-template-columns: 250px 1fr;
>   grid-template-rows: auto 1fr auto;
>   grid-template-areas:
>     "header header"
>     "nav    main"
>     "footer footer";
>   min-height: 100vh;
> }
> header { grid-area: header; }
> nav    { grid-area: nav; }
> main   { grid-area: main; }
> footer { grid-area: footer; }
> ```

> [!question]- ¿Cómo haces que un elemento ocupe de la columna 1 a la 3 (dos columnas)?
> `grid-column: 1 / 3;` (o `grid-column: span 2;`).

> [!question]- ¿Cómo centras un hijo en ambos ejes con grid en una línea?
> `display: grid; place-items: center;` en el contenedor.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace `grid-template-columns: repeat(auto-fill, minmax(200px, 1fr))`?
> Crea tantas columnas de al menos 200px como quepan y reparte el espacio sobrante entre ellas: una cuadrícula responsiva sin media queries.

> [!question]- ¿Qué diferencia hay entre `auto-fill` y `auto-fit`?
> Con pocos elementos, `auto-fill` mantiene las columnas vacías; `auto-fit` las colapsa y los elementos existentes se estiran para ocupar el ancho.

> [!question]- ¿Para qué sirve `grid-template-areas`?
> Para nombrar zonas (`"header header" "nav main"`) y colocar hijos con `grid-area: main`. Facilita leer el layout y cambiarlo en una media query.

## Práctica

- [[Lab 1 - Página de registro accesible y responsiva]]

