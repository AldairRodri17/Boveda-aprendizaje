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
# CSS - variables

Parte de: [[04-JavaScript-Web/01 HTML y CSS/_Índice HTML y CSS|HTML y CSS]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Las **variables CSS** (oficialmente *custom properties*) son propiedades cuyo nombre empieza con `--` y que guardan un valor reutilizable: `--acento: #1a73e8;`. Se leen con `var(--acento)`. Sirven para centralizar colores, espacios y tipografía (los *design tokens*), para temas claro/oscuro y para que JavaScript cambie estilos sin tocar cada regla.

### Cómo funciona
Una custom property es una **propiedad CSS real**: participa en la cascada, se **hereda** a los hijos y su valor se resuelve **en el navegador, en tiempo de ejecución**, para cada elemento.

```css
:root {                 /* :root = <html>; valores globales */
  --acento: #1a73e8;
  --espacio: 1rem;
  --radio: 8px;
}
.boton {
  background: var(--acento);
  padding: calc(var(--espacio) / 2) var(--espacio);
  border-radius: var(--radio);
}
.zona-peligro { --acento: #b00020; } /* dentro de esta zona, los botones son rojos */
```

**Valor de respaldo**: `var(--espacio, 8px)` usa `8px` si `--espacio` no está definida en ese elemento (ni heredada). El respaldo puede ser otra variable: `var(--primario, var(--acento))`.

**Diferencia con Sass**. Una variable de Sass (`$color`) existe solo al **compilar**: Sass la sustituye por su valor y en el CSS final desaparece. No se hereda, no cambia por media query ni la puede leer JavaScript. Una variable CSS vive en el navegador: cambia por selector, por media query, por `:hover` o desde JS, y todos los elementos que la usan se actualizan solos. Puedes usar ambas: Sass para lógica de compilación, custom properties para lo que cambia en ejecución.

### Tema claro/oscuro
Define los colores como variables y cambia sus valores, no las reglas:

```css
:root {
  color-scheme: light dark;
  --fondo: #ffffff;
  --texto: #1f2328;
  --borde: #d0d7de;
}
@media (prefers-color-scheme: dark) {
  :root { --fondo: #0d1117; --texto: #e6edf3; --borde: #30363d; }
}
/* Selección manual del usuario: gana sobre la preferencia del sistema */
:root[data-tema="oscuro"] { --fondo: #0d1117; --texto: #e6edf3; --borde: #30363d; }
:root[data-tema="claro"]  { --fondo: #ffffff; --texto: #1f2328; --borde: #d0d7de; }

body { background: var(--fondo); color: var(--texto); }
```

`color-scheme` le dice al navegador que también adapte controles nativos y barras de scroll.

### Leer y cambiar desde JavaScript
```js
const raiz = document.documentElement;

// Leer el valor calculado (incluye lo que venga de hojas de estilo)
const acento = getComputedStyle(raiz).getPropertyValue('--acento').trim();

// Cambiar: se aplica como estilo en línea de <html>
raiz.style.setProperty('--acento', '#0a7d32');

// Cambiar tema y recordarlo (ver localStorage)
function ponerTema(tema) {
  raiz.dataset.tema = tema;
  localStorage.setItem('tema', tema);
}
```

`getPropertyValue` devuelve una cadena (puede traer espacios al inicio); `style.getPropertyValue` solo ve lo que se puso en línea. Guardar la preferencia se ve en [[localStorage]].

### `@property`
Con `@property` registras una variable con tipo, valor inicial y si se hereda. Así el navegador sabe que es un `<color>` o un `<length>` y puede **animarla** con transiciones:

```css
@property --progreso { syntax: '<percentage>'; inherits: false; initial-value: 0%; }
```

### Cuándo usarlo y cuándo no
Úsalas para tokens de diseño (colores, espacios, radios, sombras), temas y valores que cambian por contexto o desde JS. Son la base de un [[Design systems]]. No las uses para todo valor único que nunca se repite: agregan una indirección que dificulta leer. Para cálculos complejos de compilación (bucles, mapas), Sass sigue siendo útil.

### Errores comunes
- **Olvidar `var()`**: `color: --acento` no funciona.
- **Unidades sueltas**: `--n: 10;` y luego `width: var(--n)px` no concatena. Usa `calc(var(--n) * 1px)`.
- **Valor inválido en tiempo de cálculo**: si la variable tiene un valor que no sirve para esa propiedad, la propiedad se vuelve `unset` (no usa la declaración anterior).
- **Nombres sensibles a mayúsculas**: `--Acento` y `--acento` son distintas.
- **Esperar que el respaldo aplique cuando la variable existe pero tiene un valor incorrecto**: el respaldo solo aplica si no está definida.

### Para profundizar
- [Usar custom properties (MDN)](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties)
- [`var()` (MDN)](https://developer.mozilla.org/en-US/docs/Web/CSS/var)
- [`@property` (MDN)](https://developer.mozilla.org/en-US/docs/Web/CSS/@property)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Escribe en JS cómo leer y cambiar `--acento` del documento.
> ```js
> const root = document.documentElement;
> getComputedStyle(root).getPropertyValue('--acento'); // leer
> root.style.setProperty('--acento', '#e91e63');       // cambiar
> ```

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre una variable CSS (`--color`) y una variable de Sass?
> La de CSS existe en el navegador en tiempo de ejecución: hereda por el DOM, se puede cambiar con JS o por media query y recalcula al momento. La de Sass se resuelve al compilar y desaparece.

> [!question]- ¿Cómo implementas tema claro/oscuro con variables?
> Define los colores en `:root` y sobrescríbelos en `@media (prefers-color-scheme: dark)` y/o en `[data-theme="dark"]`; los componentes solo usan `var(--fondo)`. Con JS cambias `document.documentElement.dataset.theme`. Añade `color-scheme: light dark` para controles nativos.

> [!question]- ¿Qué hace el segundo argumento en `var(--espacio, 8px)`?
> Es el valor de respaldo si `--espacio` no está definida en ese elemento.

## Práctica

- [[Lab 1 - Página de registro accesible y responsiva]]
- [[Lab 6 - Web Component con Shadow DOM]]

