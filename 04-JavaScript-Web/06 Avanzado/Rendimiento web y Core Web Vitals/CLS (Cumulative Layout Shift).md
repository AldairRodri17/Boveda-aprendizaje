---
proyecto: Conocimiento
tipo: concepto
nivel: avanzado
progreso: pendiente
estado: pendiente
area: JavaScript y Web
tags: [javascript-web]
---
# CLS (Cumulative Layout Shift)

Parte de: [[Rendimiento web y Core Web Vitals]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**CLS** (*Cumulative Layout Shift*) mide la **estabilidad visual** de una página: cuánto se mueve inesperadamente el contenido visible mientras el usuario la usa. Es la métrica de las [[Rendimiento web y Core Web Vitals|Core Web Vitals]] que captura la experiencia frustrante de ir a tocar un botón y que, en el último momento, una imagen o un anuncio empuje todo hacia abajo y termines tocando otra cosa.

Umbrales (percentil 75): **bueno ≤ 0.1**, necesita mejora entre 0.1 y 0.25, **malo > 0.25**. No tiene unidades: es un puntaje.

### Cómo funciona
#### Puntaje de un desplazamiento
Cada vez que un elemento visible cambia de posición inicial entre dos cuadros, el navegador registra un *layout shift* con un puntaje:

**puntaje = fracción de impacto × fracción de distancia**

- **Fracción de impacto**: qué parte del viewport ocupan, sumadas, el área donde estaban y donde quedaron los elementos que se movieron.
- **Fracción de distancia**: cuánto se movieron, como proporción de la dimensión mayor del viewport.

Ejemplo: un bloque que ocupa la mitad del viewport baja un 25 % de la altura. El área afectada (antes y después) cubre 0.75 del viewport, y la distancia es 0.25: puntaje = 0.75 × 0.25 ≈ 0.19, que ya está fuera de "bueno" con un solo salto.

#### Ventanas de sesión
Los desplazamientos se agrupan en **ventanas de sesión**: ráfagas de cambios separados por menos de 1 segundo, con una duración máxima de 5 segundos por ventana. El CLS de la página es la **ventana con mayor suma**, no el total de toda la visita; así una página que se usa por horas no se castiga solo por durar mucho.

#### Cambios esperados no cuentan
Los desplazamientos que ocurren dentro de los **500 ms posteriores a una interacción discreta** del usuario (clic, toque, tecla) se marcan con `hadRecentInput` y **no suman**: si el usuario hace clic en "Mostrar más" y el contenido se expande, es lo que esperaba. El scroll y los movimientos del puntero no cuentan como esa interacción. Tampoco cuentan las animaciones con `transform`, porque no cambian la posición de layout.

```mermaid
flowchart LR
    S1[shift 0.05] --> S2[shift 0.08 a 0.4 s]
    S2 --> G[pausa mayor a 1 s]
    G --> S3[shift 0.02]
    S1 -.Ventana 1 = 0.13.-> V[CLS = máximo de ventanas = 0.13]
    S3 -.Ventana 2 = 0.02.-> V
```

#### Causas típicas
- **Imágenes, videos o iframes sin dimensiones**: el navegador no sabe cuánto espacio reservar hasta que llegan.
- **Anuncios, banners de cookies y widgets** que se insertan arriba del contenido.
- **Contenido que llega por `fetch`** y se inserta empujando lo que ya estaba (avisos, recomendaciones).
- **Fuentes web** que, al cargar, cambian el tamaño del texto (FOUT con métricas muy distintas).
- **Animar propiedades de layout** (`top`, `height`, `margin`) en lugar de `transform`.

### Ejemplo
```html
<!-- MAL: sin dimensiones, el texto de abajo brinca cuando llega la imagen -->
<img src="/img/producto.webp" alt="Termo de acero">

<!-- BIEN: el navegador reserva el espacio con la proporción 4:3 -->
<img src="/img/producto.webp" alt="Termo de acero" width="800" height="600"
     style="max-width:100%; height:auto">
```

Espacio reservado para contenido que llega después (aviso de promociones en Itzamna.Clio):

```css
.contenedor-aviso { min-height: 56px; }               /* el hueco existe desde el inicio */
.zona-anuncio { aspect-ratio: 728 / 90; width: 100%; }
.esqueleto { height: 1.2em; background: #eee; border-radius: 4px; }
```

```js
// Inserta notificaciones como overlay en lugar de empujar el contenido
aviso.classList.add('toast');   // position: fixed; no afecta al layout del resto
```

Fuentes con métricas ajustadas para minimizar el salto al cargar:

```css
@font-face {
  font-family: 'Marca';
  src: url('/fuentes/marca.woff2') format('woff2');
  font-display: swap;
}
@font-face {               /* respaldo con métricas parecidas a la fuente final */
  font-family: 'Marca Respaldo';
  src: local('Arial');
  size-adjust: 105%;
}
body { font-family: 'Marca', 'Marca Respaldo', sans-serif; }
```

Ver qué se movió: en DevTools, panel **Performance**, la pista *Layout shifts* resalta los elementos; o en la consola:

```js
new PerformanceObserver((lista) => {
  for (const e of lista.getEntries()) {
    if (!e.hadRecentInput) console.log('shift', e.value.toFixed(3), e.sources?.map((s) => s.node));
  }
}).observe({ type: 'layout-shift', buffered: true });
```

### Cuándo usarlo y cuándo no
- **Siempre** pon `width` y `height` (o `aspect-ratio`) en imágenes y medios: es la corrección de mayor impacto.
- Reserva espacio para todo lo que llega tarde, o colócalo debajo del contenido visible o como overlay.
- Para animaciones, usa `transform`.
- En SPAs, CLS se sigue midiendo durante toda la visita (no solo en la carga), así que importan también los cambios al navegar entre vistas y al cargar datos.

### Errores comunes
- **Esqueletos de altura distinta** al contenido real: también producen salto al reemplazarse.
- **Banners de cookies insertados arriba** que empujan la página; mejor fijos abajo como overlay.
- **Creer que todo cambio cuenta**: los que siguen a un clic del usuario dentro de 500 ms no cuentan.
- **Medir solo la carga en Lighthouse**: muchos saltos ocurren después (scroll, datos tardíos) y solo se ven en campo.
- **`height: auto` sin atributos `width`/`height`** en el HTML: el CSS no basta para reservar si el navegador no conoce la proporción.

### Para profundizar
- [web.dev: Cumulative Layout Shift (CLS)](https://web.dev/articles/cls)
- [web.dev: Optimize CLS](https://web.dev/articles/optimize-cls)
- [MDN: LayoutShift](https://developer.mozilla.org/en-US/docs/Web/API/LayoutShift)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué mide CLS y cuál es el umbral "bueno"?
> Cuánto se desplaza inesperadamente el contenido visible (impacto × distancia, agrupado en ventanas). Bueno: ≤ 0.1; malo: > 0.25.

> [!question]- ¿Qué causas típicas tiene?
> Imágenes/iframes sin dimensiones, anuncios o banners que se insertan arriba, fuentes web que cambian de tamaño al cargar y contenido inyectado sobre lo ya visible.

> [!question]- ¿Cuenta como layout shift un cambio justo después de que el usuario hace clic?
> No: los cambios dentro de los 500 ms posteriores a una interacción del usuario se excluyen (se consideran esperados).

## Práctica

- [[Lab 8 - Medir Core Web Vitals con DevTools y Lighthouse]]

