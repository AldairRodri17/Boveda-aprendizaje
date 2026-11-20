---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: JavaScript y Web
herramienta: VS Code, navegador, DevTools y la API pública de prueba JSONPlaceholder
tags: [javascript-web, laboratorio]
---
# Lab 4 - fetch con AbortController y manejo de errores

Parte de: [[04-JavaScript-Web/03 Asincronía en JS/_Índice Asincronía en JS|Asincronía en JS]]

**Cubre:** [[fetch]] · [[AbortController]] · [[Promise.all, allSettled y race]] · [[async y await]] · [[Manejo de errores en JS]] · [[Debounce y throttle]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Crear un buscador de publicaciones que consulte una API mientras escribes, cancele la petición anterior, tenga tiempo límite y muestre errores HTTP y de red de forma clara.

## Pasos

1. Crea `buscador.html` y `buscador.js` (módulo) en esta carpeta.
2. Escribe `pedirJson(url, { signal })` que lance un `HttpError` propio (con `status`) cuando `res.ok` sea falso.
3. Un `<input>` llama a buscar con debounce de 300 ms usando `https://jsonplaceholder.typicode.com/posts?q=<texto>`.
4. Cada búsqueda nueva aborta la anterior con un `AbortController`; las cancelaciones no deben mostrar error.
5. Combina el `signal` de la búsqueda con `AbortSignal.timeout(5000)` usando `AbortSignal.any`.
6. Muestra estados: *Cargando…*, resultados, *Sin resultados*, *Error 404* (prueba con una URL mala) y *Sin conexión* (DevTools → Network → Offline).
7. Agrega un botón "Cargar usuario 1 y sus publicaciones" que pida `/users/1` y `/posts?userId=1` en paralelo con `Promise.all`, y otra versión con `allSettled` donde una URL falle a propósito.
8. En Network, con *Slow 3G*, escribe rápido y verifica que las peticiones anteriores salen como `(canceled)`.

## Criterios de terminado

- [ ] Escribir rápido varias letras deja solo una petición completada; las anteriores aparecen canceladas en Network.
- [ ] Con una URL inexistente se muestra "Error 404" (no un error genérico).
- [ ] En modo Offline se muestra "Sin conexión" y la consola no tiene errores sin manejar.
- [ ] La versión con `allSettled` muestra lo que sí cargó y un aviso de lo que falló.
- [ ] El código no tiene `catch` vacíos.

## Solución

> [!success]- pedirJson y búsqueda
> ```js
> class HttpError extends Error {
>   constructor(status) { super(`Error ${status}`); this.name = 'HttpError'; this.status = status; }
> }
> async function pedirJson(url, { signal } = {}) {
>   const res = await fetch(url, { signal });
>   if (!res.ok) throw new HttpError(res.status);
>   return res.json();
> }
>
> let actual;
> async function buscar(texto) {
>   actual?.abort();
>   actual = new AbortController();
>   const signal = AbortSignal.any([actual.signal, AbortSignal.timeout(5000)]);
>   pintar({ estado: 'cargando' });
>   try {
>     const datos = await pedirJson(`https://jsonplaceholder.typicode.com/posts?q=${encodeURIComponent(texto)}`, { signal });
>     pintar({ estado: datos.length ? 'ok' : 'vacio', datos });
>   } catch (e) {
>     if (e.name === 'AbortError') return;            // cancelada por una búsqueda nueva
>     if (e.name === 'TimeoutError') return pintar({ estado: 'error', msg: 'Tardó demasiado' });
>     if (e instanceof HttpError) return pintar({ estado: 'error', msg: e.message });
>     pintar({ estado: 'error', msg: 'Sin conexión' }); // TypeError de red
>   }
> }
> ```

> [!success]- Paralelo con all y allSettled
> ```js
> const [usuario, posts] = await Promise.all([
>   pedirJson('https://jsonplaceholder.typicode.com/users/1'),
>   pedirJson('https://jsonplaceholder.typicode.com/posts?userId=1'),
> ]);
>
> const r = await Promise.allSettled([
>   pedirJson('https://jsonplaceholder.typicode.com/users/1'),
>   pedirJson('https://jsonplaceholder.typicode.com/no-existe'),
> ]);
> const fallidas = r.filter(x => x.status === 'rejected').map(x => x.reason.message);
> ```

## Notas personales

_Qué me costó, errores que cometí, dudas._
