---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Domótica y casa inteligente
tags: [indice, domotica]
---
# Hazlo tú mismo

Parte de: [[62-Domotica/_Índice Domótica y casa inteligente|Domótica y casa inteligente]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[ESPHome]] ⭐
- [[Sensores propios con ESP32]]
- [[Cajas impresas en 3D para sensores]]
- [[MQTT]]

## Laboratorios

- [[Lab 2 - Sensor de temperatura con ESP32 y ESPHome]]

## Progreso

```base
filters:
  and:
    - file.inFolder("62-Domotica/04 Hazlo tú mismo")
    - file.name != "_Índice Hazlo tú mismo"
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
