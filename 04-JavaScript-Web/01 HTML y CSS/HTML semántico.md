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
# HTML semántico

Parte de: [[04-JavaScript-Web/01 HTML y CSS/_Índice HTML y CSS|HTML y CSS]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
HTML semántico es usar cada etiqueta por su **significado** y no por su apariencia: `<nav>` para navegación, `<button>` para acciones, `<h2>` para un subtítulo, `<table>` para datos tabulares. La apariencia se resuelve con CSS; el HTML describe **qué es** cada cosa.

Ganas:
- **Accesibilidad**: los lectores de pantalla anuncian roles ("navegación", "botón", "encabezado nivel 2") y permiten saltar entre regiones y títulos. Ver [[Accesibilidad web]].
- **Comportamiento gratis**: un `<button>` es enfocable, responde a Enter y Espacio y envía formularios; un `<a href>` se abre en otra pestaña con clic medio.
- **SEO**: los buscadores entienden mejor la estructura y los títulos.
- **Mantenimiento**: el código se lee solo; `<header>` dice más que `<div class="top">`.
- **Modo lectura** del navegador y extracción de contenido funcionan mejor.

### Cómo funciona
El navegador asigna a cada elemento un **rol implícito** en el árbol de accesibilidad. Algunos elementos forman **regiones** (*landmarks*):

| Elemento | Rol implícito | Uso |
|---|---|---|
| `<header>` (hijo de body) | banner | Encabezado del sitio |
| `<nav>` | navigation | Bloque de enlaces de navegación |
| `<main>` | main | Contenido principal, único de la página |
| `<aside>` | complementary | Contenido relacionado pero secundario |
| `<footer>` (hijo de body) | contentinfo | Pie del sitio |
| `<section>` con nombre | region | Sección temática |
| `<form>` con nombre | form | Formulario |

**`<article>` vs `<section>`**
- **`<article>`**: contenido **autocontenido** que tendría sentido fuera de la página (sindicado, compartido, reutilizado): una entrada de blog, un comentario, una tarjeta de producto, un pedido en un listado.
- **`<section>`**: agrupa contenido **temático** dentro de algo mayor y normalmente lleva un encabezado: "Datos fiscales", "Historial de pagos". Si solo necesitas un contenedor para estilos, usa `<div>`.

Prueba rápida: si lo puedes imaginar en un lector RSS o en otra página tal cual, es `<article>`.

**Encabezados**: `<h1>`…`<h6>` forman el índice de la página; los usuarios de lectores de pantalla navegan por ellos. No saltes niveles (de `h2` a `h4`) y no elijas el nivel por el tamaño de letra.

**¿Cuántos `<main>` y `<h1>`?** Un solo `<main>` visible por página (la especificación permite otros solo si están ocultos con `hidden`). Para `<h1>`, la práctica recomendada es **uno** que describa la página; el algoritmo de "outline" que hacía que cada `<section>` reiniciara niveles nunca lo implementaron los navegadores, así que varios `<h1>` confunden la jerarquía.

### Ejemplo
Pantalla de pedidos de Itzamna.Clio:

```html
<body>
  <header>
    <a href="/">Clio</a>
    <nav aria-label="Principal">
      <ul>
        <li><a href="/pedidos" aria-current="page">Pedidos</a></li>
        <li><a href="/clientes">Clientes</a></li>
      </ul>
    </nav>
  </header>

  <main>
    <h1>Pedidos</h1>
    <section aria-labelledby="pend">
      <h2 id="pend">Pendientes de surtir</h2>
      <article class="pedido">
        <h3>Pedido 1024</h3>
        <p>Cliente: Ferretería López · <time datetime="2024-11-15">15 nov</time></p>
        <button type="button">Surtir</button>
      </article>
    </section>
  </main>

  <footer><small>© Itzamna</small></footer>
</body>
```

### El problema de `<div onclick>`
```html
<div class="boton" onclick="guardar()">Guardar</div>  <!-- mal -->
<button type="button" onclick="guardar()">Guardar</button> <!-- bien -->
```

Con el `div` pierdes: el **foco** con Tab, la activación con **Enter y Espacio**, el **rol** "botón" (el lector lo anuncia como texto), el estado **deshabilitado** con `disabled`, y el envío de formularios. Recuperarlo requiere `role="button"`, `tabindex="0"` y manejadores de teclado, es decir, reconstruir a mano lo que ya existía. Si navega a otra URL, es un `<a href>`; si ejecuta una acción, es un `<button>`.

### Cuándo usarlo y cuándo no
Siempre. `<div>` y `<span>` siguen siendo válidos cuando no existe un elemento con significado (contenedores de layout). Otros elementos semánticos útiles: `<time>`, `<figure>`/`<figcaption>`, `<dl>` para pares término-valor (perfecto para "detalle de pedido"), `<address>`, `<mark>`, `<details>`/`<summary>` para acordeones nativos y [[Elemento dialog]] para modales.

### Errores comunes
- **Elegir encabezados por tamaño**: usa CSS para el tamaño y el nivel según la jerarquía.
- **`<section>` como `<div>` genérico** sin encabezado.
- **Tablas para maquetar** o divs para datos tabulares: ambos confunden a los lectores.
- **Varios `<nav>` sin nombre**: ponles `aria-label` ("Principal", "Pie") para distinguirlos.
- **Enlaces sin `href`** usados como botones.

### Para profundizar
- [Elementos HTML (MDN)](https://developer.mozilla.org/en-US/docs/Web/HTML/Element)
- [Secciones y esquema en la especificación HTML (WHATWG)](https://html.spec.whatwg.org/multipage/sections.html)
- [Regiones (landmarks) en la APG](https://www.w3.org/WAI/ARIA/apg/practices/landmark-regions/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es HTML semántico y qué gana tu página al usarlo?
> Usar la etiqueta que describe el **significado** del contenido (`<header>`, `<nav>`, `<main>`, `<article>`, `<button>`) en vez de `<div>` genéricos. Gana accesibilidad (lectores de pantalla navegan por regiones y roles), SEO y código más fácil de leer.

> [!question]- ¿Cuándo usarías `<article>` y cuándo `<section>`?
> `<article>`: contenido que tiene sentido por sí solo y se podría redistribuir (una publicación, una tarjeta de producto). `<section>`: agrupa una parte temática de un documento y normalmente lleva su propio encabezado. Si solo necesitas un contenedor para estilos, usa `<div>`.

> [!question]- Error común: `<div onclick=...>` como botón. ¿Qué se pierde?
> Un `<div>` no recibe foco con Tab, no se activa con Enter/Espacio y el lector de pantalla no lo anuncia como botón. `<button type="button">` trae todo eso gratis.

> [!question]- ¿Cuántos `<main>` y cuántos `<h1>` debería tener una página?
> Un solo `<main>` visible por página. Para `<h1>` lo recomendable es uno (el título principal) y una jerarquía de encabezados sin saltos (h1 → h2 → h3).

## Práctica

- [[Lab 1 - Página de registro accesible y responsiva]]

