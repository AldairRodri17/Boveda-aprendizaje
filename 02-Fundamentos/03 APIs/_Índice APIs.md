---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Fundamentos
tags: [indice, fundamentos]
---
# APIs

Parte de: [[02-Fundamentos/_Índice Fundamentos|Fundamentos]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[REST]]
- [[Modelo de madurez de Richardson]]
- [[Diseño de endpoints y nombres de recursos]]
- [[ProblemDetails]]
- [[OpenAPI con Swagger y Scalar]]
- [[Contract-first y diseño de contratos]]
- [[gRPC]]
- [[GraphQL]]
- [[Mensajería y colas (conceptos)]]

## Laboratorios

- [[Lab 1 - Explorar un API REST con OpenAPI]]

## Progreso

```base
filters:
  and:
    - file.inFolder("02-Fundamentos/03 APIs")
    - file.name != "_Índice APIs"
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
