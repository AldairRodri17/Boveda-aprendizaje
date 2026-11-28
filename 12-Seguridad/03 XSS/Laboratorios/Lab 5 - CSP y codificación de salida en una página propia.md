---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Seguridad
herramienta: VS Code, Node.js o .NET SDK para servir la página, DevTools del navegador
tags: [seguridad, laboratorio]
contenido: completo
---
# Lab 5 - CSP y codificación de salida en una página propia

Parte de: [[12-Seguridad/03 XSS/_Índice XSS|XSS]]

**Cubre:** [[XSS]] · [[Content Security Policy]] · [[XSS - textContent vs innerHTML]] · [[XSS - codificación de salida según el contexto]] · [[XSS - escape de datos del API en templates de JS]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
En una página tuya que muestra comentarios de un JSON local, comprobar que `innerHTML` interpreta etiquetas, corregirlo con `textContent`, y agregar una CSP que bloquee scripts inline como segunda capa.

## Pasos
1. Crea `index.html`, `app.js` y `comentarios.json`. Uno de los comentarios debe contener texto con etiquetas HTML inofensivas, por ejemplo `<b>negritas</b> y <i>cursiva</i>`.
2. En `app.js` muestra los comentarios con `innerHTML` y un template literal. Sirve la página (`npx serve` o `dotnet serve`) y confirma que el texto aparece en negritas: el navegador está interpretando el dato como HTML.
3. Corrige el render con `createElement` + `textContent`. Ahora debe verse el texto literal con los signos `<` y `>`.
4. Agrega un `<script>` inline con `console.log('inline')` al HTML.
5. Sirve la página con el header `Content-Security-Policy: default-src 'self'; script-src 'self'; object-src 'none'; base-uri 'self'; frame-ancestors 'none'` (o con `<meta>` sin `frame-ancestors`).
6. Revisa la consola: el script inline debe estar bloqueado y `app.js` debe seguir funcionando.
7. Prueba primero la política en modo `Content-Security-Policy-Report-Only` y compara los mensajes.

## Criterios de terminado
- [ ] Con `innerHTML` el comentario se ve formateado; con `textContent` se ve literal.
- [ ] La consola muestra un error de CSP por el script inline y no imprime `inline`.
- [ ] `app.js` (externo) sigue cargando los comentarios.
- [ ] En modo Report-Only el script inline sí corre, pero aparece el aviso de violación.
- [ ] Explicas por qué `frame-ancestors` no funciona en `<meta>`.

## Solución

> [!success]- Render seguro
> ```js
> const res = await fetch('comentarios.json');
> const lista = document.querySelector('#lista');
> for (const c of await res.json()) {
>   const li = document.createElement('li');
>   const autor = document.createElement('strong');
>   autor.textContent = c.autor;
>   li.append(autor, ': ', c.texto); // append con strings crea nodos de texto
>   lista.append(li);
> }
> ```

> [!success]- Servir con header (Node)
> ```js
> // server.mjs
> import http from 'node:http'; import fs from 'node:fs/promises'; import path from 'node:path';
> const csp = "default-src 'self'; script-src 'self'; object-src 'none'; base-uri 'self'; frame-ancestors 'none'";
> const tipos = { '.html': 'text/html', '.js': 'text/javascript', '.json': 'application/json' };
> http.createServer(async (req, res) => {
>   const f = req.url === '/' ? 'index.html' : path.basename(req.url);
>   try {
>     const body = await fs.readFile(f);
>     res.writeHead(200, { 'Content-Type': tipos[path.extname(f)] ?? 'text/plain', 'Content-Security-Policy': csp });
>     res.end(body);
>   } catch { res.writeHead(404); res.end(); }
> }).listen(8080);
> ```
> `path.basename` evita que la URL salga de la carpeta (ver [[Path traversal]]).

> [!success]- Por qué frame-ancestors no va en meta
> La especificación de CSP ignora `frame-ancestors`, `report-uri` y `sandbox` cuando vienen en `<meta>`, porque la decisión de incrustar debe tomarse antes de que el navegador procese el HTML. Debe enviarse como header.

## Notas personales

_Qué me costó, errores que cometí, dudas._
