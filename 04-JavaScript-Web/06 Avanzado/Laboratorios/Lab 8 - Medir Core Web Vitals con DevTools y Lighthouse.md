---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: JavaScript y Web
herramienta: Chrome DevTools (Performance y Lighthouse)
tags: [javascript-web, laboratorio]
contenido: completo
---
# Lab 8 - Medir Core Web Vitals con DevTools y Lighthouse

Parte de: [[04-JavaScript-Web/06 Avanzado/_Índice Avanzado|Avanzado]]

**Cubre:** [[Rendimiento web y Core Web Vitals]] · [[LCP (Largest Contentful Paint)]] · [[INP (Interaction to Next Paint)]] · [[CLS (Cumulative Layout Shift)]] · [[Depuración con DevTools]] · [[Imágenes responsive]] · [[Pipeline de renderizado del navegador]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Crear una página con problemas de rendimiento a propósito, medir LCP, INP y CLS, corregirlos uno por uno y documentar el antes y el después.

## Pasos

1. Crea `lenta.html` con: una imagen grande (> 1 MB, sin `width`/`height` y con `loading="lazy"`) como portada, un `<script>` sin `defer` en `<head>` que haga un bucle de 1 s, un banner que se inserta con JS arriba del contenido a los 1.5 s, y un botón cuyo clic ejecute un bucle de 400 ms antes de cambiar un texto.
2. Sírvela con Live Server. En DevTools → Lighthouse, modo *Navigation*, móvil: guarda el reporte (LCP, TBT, CLS, puntuación).
3. En DevTools → Performance, graba una recarga: identifica el elemento LCP y las tareas largas (triángulo rojo). Activa *Layout shifts* para ver qué se movió.
4. Graba en Performance mientras haces clic en el botón y anota el INP de la interacción (en el panel de métricas en vivo o la pista *Interactions*).
5. Corrige: imagen redimensionada y en WebP/AVIF con `width`/`height`, sin `lazy` y con `fetchpriority="high"`; script con `defer`; reserva espacio al banner (`min-height`) o insértalo abajo; divide el trabajo del clic (actualiza el texto primero y difiere el bucle con `setTimeout`).
6. Vuelve a medir con Lighthouse y Performance. Escribe `resultados.md` con una tabla antes/después.

## Criterios de terminado

- [ ] `resultados.md` tiene LCP, TBT, CLS e INP antes y después.
- [ ] Después de corregir: LCP ≤ 2.5 s y CLS ≤ 0.1 en Lighthouse (móvil).
- [ ] La interacción del botón muestra INP ≤ 200 ms en Performance.
- [ ] Sabes señalar en la grabación de Performance qué tarea larga causaba cada problema.

## Solución

> [!success]- Correcciones típicas
> ```html
> <head>
>   <link rel="preload" as="image" href="portada-1200.avif" fetchpriority="high">
>   <script src="app.js" defer></script>
> </head>
> <img src="portada-1200.avif" width="1200" height="600" alt="..." fetchpriority="high">
> <div class="banner" style="min-height: 80px"></div>
> ```
> ```js
> boton.addEventListener('click', () => {
>   texto.textContent = 'Procesando…';           // respuesta visual inmediata
>   setTimeout(() => { trabajoPesado(); texto.textContent = 'Listo'; }); // después del pintado
> });
> ```
> Si el trabajo es inevitable, divídelo en trozos o muévelo a un Web Worker.

## Notas personales

_Qué me costó, errores que cometí, dudas._
