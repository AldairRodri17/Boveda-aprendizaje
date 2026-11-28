---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Electrónica y robótica
tags: [indice, electronica]
contenido: completo
---
# Robótica

Parte de: [[63-Electronica-y-Robotica/_Índice Electrónica y robótica|Electrónica y robótica]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Robot móvil básico]] ⭐
- [[Control PID (conceptos)]]
- [[Robot seguidor de línea]]
- [[Brazo robótico sencillo]]

## Laboratorios

- [[Lab 3 - Robot móvil con sensor de distancia]]

## Progreso

```base
filters:
  and:
    - file.inFolder("63-Electronica-y-Robotica/07 Robótica")
    - file.name != "_Índice Robótica"
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
