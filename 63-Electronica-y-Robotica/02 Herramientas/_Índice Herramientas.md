---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Electrónica y robótica
tags: [indice, electronica]
---
# Herramientas

Parte de: [[63-Electronica-y-Robotica/_Índice Electrónica y robótica|Electrónica y robótica]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Multímetro]] ⭐
- [[Protoboard]]
- [[Soldadura con cautín]] ⭐
- [[Fuente de alimentación de laboratorio]]
- [[Osciloscopio (conceptos)]]

## Laboratorios

- [[Lab 2 - Mi primera soldadura]]

## Progreso

```base
filters:
  and:
    - file.inFolder("63-Electronica-y-Robotica/02 Herramientas")
    - file.name != "_Índice Herramientas"
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
