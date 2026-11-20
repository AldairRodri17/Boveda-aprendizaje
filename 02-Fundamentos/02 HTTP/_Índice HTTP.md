---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Fundamentos
tags: [indice, fundamentos]
---
# HTTP

Parte de: [[02-Fundamentos/_Índice Fundamentos|Fundamentos]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Verbos HTTP]]
- [[GET]]
- [[POST]]
- [[PUT]]
- [[PATCH]]
- [[DELETE]]
- [[HEAD y OPTIONS]]
- [[Métodos seguros e idempotentes]]
- [[Códigos de estado HTTP]]
  - [[Códigos HTTP 1xx (informativos)]]
  - [[Códigos HTTP 2xx (éxito)]]
  - [[Códigos HTTP 3xx (redirección)]]
  - [[Códigos HTTP 4xx (error del cliente)]]
  - [[Códigos HTTP 5xx (error del servidor)]]
- [[Headers HTTP]]
- [[Content-Type y Accept]]
- [[Parámetros de ruta, query string y body]]
- [[Caché HTTP (Cache-Control y ETag)]]
- [[Compresión HTTP]]
- [[Redirecciones]]
- [[Subida de archivos (multipart)]]
- [[Paginación, filtrado y ordenamiento]]
- [[Versionado de APIs]]
- [[Compatibilidad hacia atrás y cambios incompatibles en APIs]]
- [[Idempotencia]]
- [[Rate limiting]]
- [[Webhooks]]
- [[Polling, long polling, SSE y WebSockets]]
  - [[Polling y long polling]]
  - [[Server-Sent Events (SSE)]]
  - [[WebSockets]]
- [[SignalR]]

## Laboratorios

- [[Lab 1 - Peticiones HTTP con curl y archivos .http]]
- [[Lab 2 - Caché, redirecciones y compresión con curl]]

## Progreso

```base
filters:
  and:
    - file.inFolder("02-Fundamentos/02 HTTP")
    - file.name != "_Índice HTTP"
views:
  - type: table
    name: Temas
    filters:
      and:
        - tipo != "laboratorio"
    order:
      - file.name
      - progreso
      - nivel
  - type: table
    name: Laboratorios
    filters:
      and:
        - tipo == "laboratorio"
    order:
      - file.name
      - progreso
```
