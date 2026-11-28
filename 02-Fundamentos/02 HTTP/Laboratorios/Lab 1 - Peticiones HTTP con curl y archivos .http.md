---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Fundamentos
herramienta: curl, VS Code con la extensión REST Client (o archivos .http de Visual Studio) y httpbin.org
tags: [fundamentos, laboratorio]
contenido: completo
---
# Lab 1 - Peticiones HTTP con curl y archivos .http

Parte de: [[02-Fundamentos/02 HTTP/_Índice HTTP|HTTP]]

**Cubre:** [[Verbos HTTP]] · [[GET]] · [[POST]] · [[PUT]] · [[PATCH]] · [[DELETE]] · [[HEAD y OPTIONS]] · [[Métodos seguros e idempotentes]] · [[Códigos de estado HTTP]] · [[Códigos HTTP 4xx (error del cliente)]] · [[Headers HTTP]] · [[Content-Type y Accept]] · [[Parámetros de ruta, query string y body]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Hacer peticiones con cada verbo, leer códigos y headers, y dejar un archivo `.http` reutilizable como documentación ejecutable.

## Requisitos
- `curl` (en PowerShell usa `curl.exe`).
- VS Code con la extensión **REST Client** (Huachao Mao), o Visual Studio 2022 que abre archivos `.http`.
- `https://httpbin.org` (servicio público que refleja lo que le mandas). Si no está disponible, córrelo local con Docker: `docker run -p 8080:80 kennethreitz/httpbin` y usa `http://localhost:8080`.

## Pasos
1. `curl -i "https://httpbin.org/get?estado=activo&page=2"`: identifica línea de estado, headers y body. ¿Dónde aparecen tus parámetros?
2. Envía un `POST` con JSON (`{"nombre":"Ana","edad":30}`) a `/post` indicando `Content-Type: application/json`. Luego envía lo mismo **sin** ese header. Compara los campos `json` y `data` de la respuesta.
3. Envía un formulario (`-d "nombre=Ana&ciudad=León"`) a `/post` y localiza el campo `form`.
4. Prueba `PUT`, `PATCH` y `DELETE` contra `/put`, `/patch` y `/delete`.
5. Llama `GET` a `/post`. ¿Qué código obtienes y qué header lo acompaña?
6. Usa `HEAD` (`curl -I`) contra `/get` y verifica que no hay body.
7. Simula un preflight: `curl -i -X OPTIONS https://httpbin.org/get -H "Origin: https://ejemplo.com" -H "Access-Control-Request-Method: PUT"` y lee los headers `Access-Control-*`.
8. Pide códigos concretos: `/status/201`, `/status/404`, `/status/429`, `/status/503`. Muestra solo el código con `-o /dev/null -w "%{http_code}\n"` (en PowerShell `-o NUL`).
9. Negociación de contenido: llama `/json` y `/xml`; luego llama `/headers` con `-H "Accept: application/xml"` y comprueba que el servidor vio tu header.
10. Crea `httpbin.http` con al menos 6 peticiones (una por verbo + una con variables `@base = https://httpbin.org`) separadas por `###` y ejecútalas desde el editor.

## Criterios de terminado
- [ ] En el paso 2 explicas por qué sin `Content-Type: application/json` el campo `json` sale `null`.
- [ ] El paso 5 devuelve `405` y tienes anotado el valor del header `Allow`.
- [ ] El `HEAD` del paso 6 devuelve `200` con `Content-Length` pero sin body.
- [ ] Los cuatro códigos del paso 8 salen exactamente `201`, `404`, `429` y `503`.
- [ ] Tu archivo `httpbin.http` corre completo desde VS Code o Visual Studio y está en tu repo de prácticas.
- [ ] Llenaste una tabla verbo → seguro / idempotente / ¿lleva body? con lo que observaste.

## Solución

> [!success]- Pasos 1–3
> ```
> curl -i "https://httpbin.org/get?estado=activo&page=2"           # aparecen en "args"
> curl -i https://httpbin.org/post -H "Content-Type: application/json" -d "{\"nombre\":\"Ana\",\"edad\":30}"
> curl -i https://httpbin.org/post -d "nombre=Ana&ciudad=León"      # aparece en "form"
> ```
> Sin `Content-Type: application/json`, curl con `-d` manda `application/x-www-form-urlencoded`; httpbin no lo interpreta como JSON: `json` queda `null` y el texto aparece como clave en `form`. El servidor confía en el header, no adivina.

> [!success]- Pasos 4–7
> `curl -X PUT ...`, `curl -X PATCH ...`, `curl -X DELETE https://httpbin.org/delete`. El `GET /post` devuelve `405 METHOD NOT ALLOWED` con `Allow: OPTIONS, POST` (o similar). `curl -I` usa `HEAD`. En el preflight, httpbin responde con `Access-Control-Allow-Origin` y `Access-Control-Allow-Methods`.

> [!success]- Paso 10: archivo .http de ejemplo
> ```http
> @base = https://httpbin.org
>
> GET {{base}}/get?estado=activo
> Accept: application/json
>
> ###
> POST {{base}}/post
> Content-Type: application/json
>
> {"nombre": "Ana", "edad": 30}
>
> ###
> PUT {{base}}/put
> Content-Type: application/json
>
> {"id": 7, "nombre": "Ana", "edad": 31}
>
> ###
> PATCH {{base}}/patch
> Content-Type: application/merge-patch+json
>
> {"edad": 32}
>
> ###
> DELETE {{base}}/delete
>
> ###
> GET {{base}}/status/404
> ```

> [!success]- Tabla esperada
> | Verbo | Seguro | Idempotente | Body en la petición |
> |---|---|---|---|
> | GET / HEAD / OPTIONS | Sí | Sí | No |
> | POST | No | No | Sí |
> | PUT | No | Sí | Sí |
> | PATCH | No | No (en general) | Sí |
> | DELETE | No | Sí | Normalmente no |

## Notas personales

_Qué me costó, errores que cometí, dudas._
