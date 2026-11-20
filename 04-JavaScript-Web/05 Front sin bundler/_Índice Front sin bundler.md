---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: JavaScript y Web
tags: [indice, javascript-web]
---
# Front sin bundler

Parte de: [[04-JavaScript-Web/_Índice JavaScript y Web|JavaScript y Web]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Molde Service-View-Handlers-Events]]
- [[lit-html]]

## Progreso

```base
filters:
  and:
    - file.inFolder("04-JavaScript-Web/05 Front sin bundler")
    - file.name != "_Índice Front sin bundler"
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
