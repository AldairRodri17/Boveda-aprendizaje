---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Carrera y liderazgo técnico
tags: [indice, carrera-liderazgo]
---
# Inglés técnico

Parte de: [[14-Carrera-Liderazgo/_Índice Carrera y liderazgo técnico|Carrera y liderazgo técnico]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Inglés técnico - plan de estudio]]
- [[Inglés técnico - leer documentación y RFCs]]
- [[Inglés técnico - escribir PRs, issues y design docs]]
- [[Inglés técnico - hablar en reuniones y entrevistas]]
- [[Vocabulario técnico en inglés]]

## Laboratorios

- [[Lab 5 - Escribir un PR y un issue en inglés]]

## Progreso

```base
filters:
  and:
    - file.inFolder("14-Carrera-Liderazgo/05 Inglés técnico")
    - file.name != "_Índice Inglés técnico"
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
