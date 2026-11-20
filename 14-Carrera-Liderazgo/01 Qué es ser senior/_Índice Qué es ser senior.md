---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Carrera y liderazgo técnico
tags: [indice, carrera-liderazgo]
---
# Qué es ser senior

Parte de: [[14-Carrera-Liderazgo/_Índice Carrera y liderazgo técnico|Carrera y liderazgo técnico]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Qué significa ser senior (alcance, autonomía, impacto y ownership)]]
- [[Escalera de carrera (junior, semi senior, senior, staff)]]
  - [[Desarrollador junior - expectativas]]
  - [[Desarrollador semi senior - expectativas]]
  - [[Desarrollador senior - expectativas]]
  - [[Staff engineer - expectativas]]
- [[Plan personal hacia senior]]

## Laboratorios

- [[Lab 1 - Mi plan de 6 meses hacia senior]]

## Progreso

```base
filters:
  and:
    - file.inFolder("14-Carrera-Liderazgo/01 Qué es ser senior")
    - file.name != "_Índice Qué es ser senior"
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
