---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Electrónica y robótica
tags: [indice, electronica]
---
# Sensores y actuadores

Parte de: [[63-Electronica-y-Robotica/_Índice Electrónica y robótica|Electrónica y robótica]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Sensores comunes (temperatura, distancia y movimiento)]]
- [[Motores DC, servos y motores a pasos]] ⭐
- [[Drivers de motor]]
- [[LEDs y tiras de LED direccionables]]

## Progreso

```base
filters:
  and:
    - file.inFolder("63-Electronica-y-Robotica/04 Sensores y actuadores")
    - file.name != "_Índice Sensores y actuadores"
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
