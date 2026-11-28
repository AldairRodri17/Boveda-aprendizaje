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
# Contraste y texto alternativo

Parte de: [[Accesibilidad web]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Son dos criterios del principio **Perceptible** de las WCAG. El **contraste** asegura que el texto y los controles se distingan de su fondo para personas con baja visión, daltonismo o en pantallas con mala luz. El **texto alternativo** (`alt`) da un equivalente en texto a las imágenes para quien no las ve: lectores de pantalla, imágenes que no cargan y buscadores.

### Cómo funciona: contraste
El contraste se mide como una razón entre la luminancia relativa del color más claro y la del más oscuro: `(L1 + 0.05) / (L2 + 0.05)`. Va de 1:1 (mismo color) a 21:1 (negro sobre blanco). Los mínimos en nivel **AA** son:

| Caso | Mínimo AA | Mínimo AAA |
|---|---|---|
| Texto normal | 4.5:1 | 7:1 |
| Texto grande (≥ 24 px normal o ≥ 18.66 px en negritas, es decir 18 pt / 14 pt) | 3:1 | 4.5:1 |
| Componentes de interfaz y gráficos (bordes de inputs, íconos, anillo de foco) | 3:1 | — |

El logotipo y el texto puramente decorativo o de controles deshabilitados quedan exentos.

Otro criterio relacionado (**1.4.1 Uso del color**, nivel A): el color no puede ser el único medio para transmitir información. Si un formulario marca los errores solo pintando el borde en rojo, una persona con daltonismo rojo-verde o un lector de pantalla no se entera. Agrega un ícono, un texto ("Campo obligatorio") y asócialo al input con `aria-describedby`.

### Cómo funciona: texto alternativo
El `alt` describe la **función o el contenido** de la imagen en su contexto, no su aspecto:

- **Imagen informativa** (una gráfica, una foto de producto): describe lo que aporta. `alt="Ventas de octubre: 20 % más que septiembre"`.
- **Imagen decorativa** (adornos, fondos): `alt=""` (vacío). El lector la ignora. Omitir el atributo es peor: muchos lectores anuncian el nombre del archivo.
- **Imagen-enlace o ícono-botón**: el texto describe la **acción**, no el dibujo. Un ícono de lupa dentro de un botón: `aria-label="Buscar"` en el botón y el SVG con `aria-hidden="true"`; si es `<img>`, `alt="Buscar"`.
- **Imagen con texto**: el `alt` repite el texto.
- **Imagen compleja** (diagrama de arquitectura): `alt` breve y descripción larga cercana en el texto o con `<figure>` y `<figcaption>`.

### Ejemplo
```html
<figure>
  <img src="reporte-ventas.png"
       alt="Gráfica de barras: ventas de octubre 20 % arriba de septiembre">
  <figcaption>Ventas mensuales 2024 (Wee.Reporteria)</figcaption>
</figure>

<img src="separador-ondas.svg" alt="">

<button type="button" aria-label="Cerrar">
  <svg aria-hidden="true" focusable="false"><!-- X --></svg>
</button>

<label for="rfc">RFC</label>
<input id="rfc" aria-invalid="true" aria-describedby="rfc-error">
<p id="rfc-error" class="error">⚠ El RFC debe tener 12 o 13 caracteres.</p>
```

```css
/* El estado de error no depende solo del color: hay ícono y texto */
.error { color: #b00020; } /* #b00020 sobre blanco supera 4.5:1 */
input[aria-invalid="true"] { border: 2px solid #b00020; }
```

Para medir: el selector de color de Chrome DevTools muestra la razón de contraste y si pasa AA/AAA; también la extensión axe o el *Colour Contrast Analyser* (gratuito). En fotografías con texto encima (portadas hechas con tu Sony a6700, por ejemplo), pon una capa oscura semitransparente debajo del texto para asegurar el contraste en toda la imagen.

### Cuándo usarlo y cuándo no
El contraste aplica a todo texto legible y a los controles. El `alt` lo lleva **toda** etiqueta `<img>`; la única decisión es si va vacío o con contenido. Las imágenes de fondo CSS (`background-image`) no se anuncian: úsalas solo para decoración; si la imagen informa, usa `<img>`.

### Errores comunes
- **`alt="imagen"` o `alt="foto123.jpg"`**: no aporta nada. Describe la función.
- **Empezar con "Imagen de…"**: el lector ya anuncia que es una imagen.
- **Texto gris claro sobre blanco** (`#999` sobre `#fff` da cerca de 2.8:1): no pasa AA. Oscurece a algo como `#767676` o más.
- **Placeholder como única etiqueta**: suele tener bajo contraste y desaparece al escribir. Usa `<label>`.
- **Errores solo en rojo**: agrega texto e ícono.

### Para profundizar
- [Entender 1.4.3 Contraste mínimo (W3C)](https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html)
- [Árbol de decisión para `alt` (W3C WAI)](https://www.w3.org/WAI/tutorials/images/decision-tree/)
- [Texto alternativo en MDN (`<img>`)](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué contraste mínimo pide WCAG AA para texto normal y texto grande?
> 4.5:1 para texto normal y 3:1 para texto grande (aprox. 24px, o 18.66px en negrita). Los componentes de interfaz y gráficos necesitan 3:1.

> [!question]- ¿Qué `alt` le pones a una imagen decorativa y a un ícono-botón?
> Decorativa: `alt=""` (vacío, para que el lector la ignore). Botón solo con ícono: el texto debe decir la **acción** ("Eliminar foto"), no describir el dibujo ("bote de basura").

> [!question]- Caso: un formulario marca errores solo en rojo. ¿Qué falla?
> Depende solo del color: quienes no distinguen el rojo no lo notan. Añade texto del error, un ícono y asócialo al campo con `aria-describedby` y `aria-invalid="true"`.

## Práctica

- [[Lab 1 - Página de registro accesible y responsiva]]

