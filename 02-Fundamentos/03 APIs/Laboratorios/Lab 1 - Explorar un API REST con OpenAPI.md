---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Fundamentos
herramienta: Swagger Petstore (petstore3.swagger.io), curl o REST Client y un editor
tags: [fundamentos, laboratorio]
contenido: completo
---
# Lab 1 - Explorar un API REST con OpenAPI

Parte de: [[02-Fundamentos/03 APIs/_Índice APIs|APIs]]

**Cubre:** [[REST]] · [[Modelo de madurez de Richardson]] · [[Diseño de endpoints y nombres de recursos]] · [[OpenAPI con Swagger y Scalar]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Leer un contrato OpenAPI real, usarlo para hacer llamadas y criticar su diseño con los criterios de REST y del modelo de Richardson.

## Requisitos
- Navegador y `curl` (o REST Client).
- Swagger Petstore v3: UI en `https://petstore3.swagger.io` y contrato en `https://petstore3.swagger.io/api/v3/openapi.json`. Es un demo público: sus datos se reinician y pueden cambiar.

## Pasos
1. Descarga `openapi.json` y ubica: versión de OpenAPI, `servers`, lista de `paths` y `components/schemas`.
2. Haz una tabla de cada endpoint: método, ruta, propósito y códigos de respuesta documentados.
3. Crea una mascota con `POST /api/v3/pet` (usa el ejemplo del esquema `Pet` con un `id` propio alto) y luego consúltala con `GET /api/v3/pet/{id}`.
4. Actualízala con `PUT /api/v3/pet` y bórrala con `DELETE /api/v3/pet/{id}`. Vuelve a consultarla.
5. Prueba un `GET /api/v3/pet/abc` (id inválido) y anota código y body.
6. Ubica el nivel de Richardson del API y justifica.
7. Redacta al menos 5 observaciones de diseño (nombres, verbos en rutas, singular/plural, dónde va el id en el `PUT`, formato de errores) y propone la versión mejorada de cada ruta.
8. Abre el mismo `openapi.json` en otra interfaz (por ejemplo `https://editor.swagger.io` con *File → Import URL*) y compara.

## Criterios de terminado
- [ ] Tu tabla incluye todos los paths del contrato con su método y códigos.
- [ ] La secuencia crear → consultar → actualizar → borrar → consultar termina en `404` (o el error documentado).
- [ ] Anotaste qué devuelve el id inválido y si el error sigue algún estándar (¿ProblemDetails?).
- [ ] Justificaste el nivel de Richardson con ejemplos concretos.
- [ ] Tienes 5 propuestas de mejora con la ruta original y la propuesta.

## Solución

> [!success]- Nivel de Richardson
> Nivel **2**: hay recursos (`/pet`, `/store/order`, `/user`) y usa verbos HTTP, pero no hay enlaces hipermedia en las respuestas. Algunos detalles lo alejan del nivel 2 "limpio" (ver abajo).

> [!success]- Observaciones de diseño esperadas
> | Original | Problema | Propuesta |
> |---|---|---|
> | `GET /pet/findByStatus?status=` | Verbo en la ruta | `GET /pets?status=available` |
> | `GET /pet/findByTags` | Verbo en la ruta | `GET /pets?tags=a,b` |
> | `/pet`, `/user` en singular | Colecciones en singular | `/pets`, `/users` |
> | `PUT /pet` con el id en el body | El recurso no se identifica por la URL | `PUT /pets/{id}` |
> | `POST /pet/{petId}` con form data para actualizar | Usa POST para modificar parcialmente | `PATCH /pets/{id}` |
> | `GET /user/login?username=&password=` | Credenciales en la query (quedan en logs) | `POST /sessions` con body, o mejor OAuth/OIDC |
> | Errores como texto o códigos genéricos | No usa un formato estándar | `application/problem+json` (ProblemDetails) |

> [!success]- Llamadas de ejemplo
> ```
> curl -i -X POST https://petstore3.swagger.io/api/v3/pet -H "Content-Type: application/json" -d "{\"id\":987654,\"name\":\"Firulais\",\"photoUrls\":[],\"status\":\"available\"}"
> curl -i https://petstore3.swagger.io/api/v3/pet/987654
> curl -i -X DELETE https://petstore3.swagger.io/api/v3/pet/987654
> ```
> Los códigos exactos que devuelve el demo pueden variar respecto a lo documentado: anotar esas diferencias también es parte del ejercicio.

## Notas personales

_Qué me costó, errores que cometí, dudas._
