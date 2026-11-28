---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Electrónica y robótica
tags: [indice, electronica]
---
# Energía

Parte de: [[63-Electronica-y-Robotica/_Índice Electrónica y robótica|Electrónica y robótica]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Baterías para proyectos electrónicos]]
- [[Reguladores de voltaje]]
- [[Paneles solares pequeños]]

## Progreso

```base
filters:
  and:
    - file.inFolder("63-Electronica-y-Robotica/06 Energía")
    - file.name != "_Índice Energía"
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
