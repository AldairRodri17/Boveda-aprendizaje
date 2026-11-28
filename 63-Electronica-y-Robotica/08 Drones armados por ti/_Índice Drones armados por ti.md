---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Electrónica y robótica
tags: [indice, electronica]
contenido: completo
---
# Drones armados por ti

Parte de: [[63-Electronica-y-Robotica/_Índice Electrónica y robótica|Electrónica y robótica]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Partes de un dron armado (frame, motores, ESC y controladora)]]
- [[Betaflight y ArduPilot (conceptos)]]
- [[FPV (conceptos y normativa)]] (enlaza con 35)

## Progreso

```base
filters:
  and:
    - file.inFolder("63-Electronica-y-Robotica/08 Drones armados por ti")
    - file.name != "_Índice Drones armados por ti"
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
