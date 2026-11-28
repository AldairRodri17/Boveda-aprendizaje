---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Hablar en público y exposiciones
tags: [indice, hablar-en-publico]
---
# Práctica y evaluación

Parte de: [[40-Hablar-en-Publico/_Índice Hablar en público y exposiciones|Hablar en público y exposiciones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Rúbrica de exposiciones]] ⭐ _(existe)_
- [[Grabarte y revisarte]] ⭐
- [[Toastmasters y clubes de oratoria]]
- [[Bitácora de presentaciones]]

## Laboratorios

- [[Lab 4 - Video grabado y revisión con la rúbrica]]

## Progreso

```base
filters:
  and:
    - file.inFolder("40-Hablar-en-Publico/08 Práctica y evaluación")
    - file.name != "_Índice Práctica y evaluación"
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
