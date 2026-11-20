---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: JavaScript y Web
tags: [javascript-web]
---
# CSS - metodologías (BEM)

Parte de: [[04-JavaScript-Web/01 HTML y CSS/_Índice HTML y CSS|HTML y CSS]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **metodología CSS** es una convención para nombrar clases y organizar estilos de modo que un proyecto grande no se vuelva un caos de reglas que se pisan. **BEM** (*Block, Element, Modifier*) es la más conocida: cada clase dice a qué componente pertenece, qué parte es y en qué variante está. El objetivo es que todas las reglas tengan **especificidad baja y uniforme** (una sola clase) y que leer el HTML te diga qué CSS lo afecta.

### Cómo funciona
- **Bloque** (*Block*): un componente independiente y reutilizable. `.tarjeta`, `.menu`, `.formulario-login`.
- **Elemento** (*Element*): una parte del bloque que no tiene sentido sola. Se une con **dos guiones bajos**: `.tarjeta__titulo`, `.tarjeta__imagen`.
- **Modificador** (*Modifier*): una variante o estado del bloque o del elemento. Se une con **dos guiones medios**: `.tarjeta--destacada`, `.boton--peligro`, `.tarjeta__titulo--grande`.

Formato: `bloque__elemento--modificador`. Las palabras dentro de un nombre se separan con un guion simple: `.formulario-login__campo-correo`.

Reglas importantes:
1. **Una clase por selector**: escribes `.tarjeta__titulo`, no `.tarjeta .tarjeta__titulo`. El nombre ya expresa la relación, así que anidar solo sube la especificidad (de 0,1,0 a 0,2,0) y obliga a usar selectores más fuertes para sobrescribir. Ver [[CSS - selectores y especificidad]].
2. **No hay "nietos"**: aunque el HTML anide, el nombre es `bloque__elemento`, nunca `bloque__elemento__subelemento`. Si una parte crece mucho, conviértela en otro bloque.
3. **El modificador acompaña a la clase base**: `class="boton boton--peligro"`, no solo `boton--peligro`.
4. **Sin estilos por etiqueta o ID** dentro de los componentes.

### Ejemplo
Una tarjeta de pedido para Itzamna.Clio:

```html
<article class="tarjeta tarjeta--destacada">
  <img class="tarjeta__imagen" src="logo-cliente.png" alt="">
  <h3 class="tarjeta__titulo">Pedido 1024</h3>
  <p class="tarjeta__texto">Entrega: 15 de noviembre</p>
  <button class="boton boton--primario tarjeta__accion" type="button">Ver detalle</button>
</article>
```

```css
.tarjeta { border: 1px solid var(--borde); border-radius: 8px; padding: 1rem; }
.tarjeta--destacada { border-color: var(--acento); }
.tarjeta__titulo { margin: 0; font-size: 1.125rem; }
.tarjeta__texto { color: var(--texto-secundario); }
.tarjeta__accion { margin-block-start: .75rem; }

.boton { padding: .5rem 1rem; border-radius: 6px; }
.boton--primario { background: var(--acento); color: white; }
```

Fíjate en el botón: es su propio bloque (`boton`) y además es un elemento de la tarjeta (`tarjeta__accion`). Eso se llama *mezcla* (*mix*): el bloque `boton` define cómo se ve y `tarjeta__accion` define cómo se coloca dentro de la tarjeta.

### Cuándo usarlo y cuándo no
BEM funciona muy bien en **CSS global sin herramientas**, como tu frontend en JavaScript ES2022 sin bundler, o en una librería de componentes compartida. Si ya tienes un mecanismo que aísla estilos, BEM aporta menos. Alternativas que resuelven el mismo problema (choques de nombres y especificidad):

- **CSS Modules**: el bundler (Vite lo soporta con `*.module.css`) genera nombres de clase únicos por archivo. Muy común en React.
- **`@scope`**: regla nativa de CSS que limita estilos a un subárbol del DOM (revisa su soporte en tu navegador objetivo).
- **Shadow DOM** en Web Components: aislamiento real de estilos (ver [[Shadow DOM]]).
- **CSS utilitario** (Tailwind y similares): clases pequeñas de una sola propiedad compuestas en el HTML.
- **Cascade layers** (`@layer`): ordenan bloques de estilos (reset, base, componentes, utilidades) para controlar quién gana sin pelear con especificidad.
- **CSS-in-JS**: estilos definidos en el componente.

También existen otras convenciones de nombres (OOCSS, SMACSS, ITCSS para ordenar archivos), pero BEM es la más extendida.

### Errores comunes
- **Anidar selectores BEM** (`.tarjeta .tarjeta__titulo`): rompe la ventaja de especificidad plana.
- **Cadenas de elementos** (`.tarjeta__cuerpo__titulo`): refleja el DOM y se rompe al reorganizar el HTML.
- **Modificador sin clase base**: pierdes los estilos comunes.
- **Usar el modificador para estados controlados por JS sin consistencia**: decide una convención (`.is-abierto` o `--abierto`) y mantenla.

### Para profundizar
- [Sitio oficial de BEM (getbem)](https://getbem.com/)
- [Documentación de la metodología BEM](https://en.bem.info/methodology/)
- [Cascade layers (MDN)](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Cascade_layers)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué significa BEM y cómo se escribe?
> Bloque, Elemento, Modificador: `.tarjeta`, `.tarjeta__titulo`, `.tarjeta--destacada`. Nombres planos con una sola clase para mantener la especificidad baja y predecible.

> [!question]- Caso: ¿escribirías `.tarjeta .tarjeta__titulo` en BEM?
> No: el elemento ya es único por su nombre; anidar sube la especificidad y acopla el estilo a la estructura. Basta `.tarjeta__titulo`.

> [!question]- ¿Qué alternativas resuelven lo mismo que BEM?
> CSS Modules (nombres con alcance local generados al compilar), Shadow DOM (encapsulación real), utilidades tipo Tailwind y `@scope`/`@layer` nativos.

