---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: JavaScript y Web
tags: [indice, javascript-web]
contenido: completo
---
# Asincronía en JS

Parte de: [[04-JavaScript-Web/_Índice JavaScript y Web|JavaScript y Web]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Callbacks]]
- [[Promesas]]
- [[async y await]]
- [[Promise.all, allSettled y race]]
- [[Event loop]]
- [[Microtareas y macrotareas]]
- [[Timers]]
- [[fetch]]
- [[AbortController]]
- [[Generadores e iteradores]]

## Laboratorios

- [[Lab 3 - Predecir el orden del event loop]]
- [[Lab 4 - fetch con AbortController y manejo de errores]]

## Progreso

```base
filters:
  and:
    - file.inFolder("04-JavaScript-Web/03 Asincronía en JS")
    - file.name != "_Índice Asincronía en JS"
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
