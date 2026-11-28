---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Fundamentos
herramienta: curl y httpbin.org (o httpbin local con Docker)
tags: [fundamentos, laboratorio]
contenido: completo
---
# Lab 2 - Caché, redirecciones y compresión con curl

Parte de: [[02-Fundamentos/02 HTTP/_Índice HTTP|HTTP]]

**Cubre:** [[Caché HTTP (Cache-Control y ETag)]] · [[Redirecciones]] · [[Compresión HTTP]] · [[Códigos HTTP 3xx (redirección)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Comprobar cómo funcionan la revalidación con `ETag`, los distintos códigos de redirección y la compresión, midiendo bytes y saltos.

## Requisitos
Igual que el Lab 1 de HTTP: `curl` y `https://httpbin.org` (o `docker run -p 8080:80 kennethreitz/httpbin`).

## Pasos
1. `curl -i https://httpbin.org/etag/v5` y anota el `ETag` que devuelve.
2. Repite con `-H 'If-None-Match: "v5"'`. Luego con `-H 'If-None-Match: "v4"'`. Compara códigos y tamaño del body.
3. Llama `/cache` normal y con `-H "If-Modified-Since: Mon, 01 Jan 2024 00:00:00 GMT"`.
4. Pide `/response-headers?Cache-Control=public,max-age=60` y confirma que el header llega en la respuesta. Escribe qué haría una CDN con eso y qué cambiarías si fuera un perfil de usuario.
5. `curl -i https://httpbin.org/redirect/3` (sin seguir) y luego con `-L`. Usa `-w "%{num_redirects} %{url_effective}\n"`.
6. Prueba `/redirect-to?url=/get&status_code=301`, luego con `302`, `307` y `308`, haciendo `curl -L -X POST -d "a=1"` en cada caso. Observa qué método llega al destino (campo `url` y si aparece `form`).
7. Compresión: compara el tamaño descargado de `/gzip` con y sin `--compressed` usando `-w "%{size_download}\n" -o /dev/null`. Revisa los headers `Content-Encoding` y `Vary`.
8. Llama `/brotli` con `-H "Accept-Encoding: br"` y `-i`: ¿qué `Content-Encoding` llega?

## Criterios de terminado
- [ ] El paso 2 devuelve `304` sin body con `"v5"` y `200` con body con `"v4"`.
- [ ] El paso 3 devuelve `304` cuando mandas `If-Modified-Since`.
- [ ] Con `-L` el paso 5 reporta `3` redirecciones y termina en `/get`.
- [ ] Tienes una tabla de 301/302/307/308 indicando qué método llegó al destino al seguir un `POST`.
- [ ] Anotaste bytes descargados con y sin compresión en el paso 7.
- [ ] Explicas cuándo usarías `public, max-age`, `private` y `no-store`.

## Solución

> [!success]- Pasos 1–3: revalidación
> Con `If-None-Match` igual al `ETag` actual, el servidor responde `304 Not Modified` sin cuerpo: el cliente reutiliza su copia. Con otro valor, `200` con el recurso completo. `/cache` responde `304` cuando la petición trae `If-Modified-Since` o `If-None-Match`, y `200` en caso contrario.

> [!success]- Paso 4: Cache-Control
> `public, max-age=60` permite que navegador y CDN la sirvan 60 s sin preguntar. Para un perfil de usuario: `private, no-cache` (o `no-store` si es sensible), para que ninguna caché compartida la guarde.

> [!success]- Paso 6: redirecciones y método
> Con curl `-L`: `301` y `302` (y `303`) cambian el `POST` a `GET` al seguir (comportamiento histórico de los navegadores que curl imita); `307` y `308` conservan `POST` y el body. Por eso para mover un endpoint de API que recibe `POST` se usa `308` (permanente) o `307` (temporal).
> | Código | Permanente | Conserva método |
> |---|---|---|
> | 301 | Sí | No (en la práctica) |
> | 302 | No | No (en la práctica) |
> | 307 | No | Sí |
> | 308 | Sí | Sí |

> [!success]- Pasos 7–8: compresión
> Sin `--compressed` curl descarga los bytes comprimidos tal cual (verás binario si los imprimes); con `--compressed` envía `Accept-Encoding` y descomprime. `size_download` mide bytes en la red. La respuesta incluye `Content-Encoding: gzip` (o `br`). Si `Vary: Accept-Encoding` no está, una caché podría servir la versión comprimida a un cliente que no la soporta.

## Notas personales

_Qué me costó, errores que cometí, dudas._
