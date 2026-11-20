---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Drones
tags: [indice, drones]
---
# Edición y mantenimiento

Parte de: [[35-Drones/_Índice Drones|Drones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Edición de material aéreo]]
- [[Mantenimiento del dron (firmware y hélices)]]

## Laboratorios

- [[Lab 5 - Edición de material aéreo en DaVinci Resolve]]

## Progreso

```base
filters:
  and:
    - file.inFolder("35-Drones/04 Edición y mantenimiento")
    - file.name != "_Índice Edición y mantenimiento"
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
