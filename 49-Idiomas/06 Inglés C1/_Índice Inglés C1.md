---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Idiomas
tags: [indice, idiomas]
---
# Inglés C1

Parte de: [[49-Idiomas/_Índice Idiomas|Idiomas]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Inglés C1 - inversión y énfasis]]
- [[Inglés C1 - expresiones idiomáticas]]
- [[Inglés C1 - registro formal e informal]] ⭐
- [[Inglés C1 - matices y lenguaje diplomático]]

## Progreso

```base
filters:
  and:
    - file.inFolder("49-Idiomas/06 Inglés C1")
    - file.name != "_Índice Inglés C1"
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
