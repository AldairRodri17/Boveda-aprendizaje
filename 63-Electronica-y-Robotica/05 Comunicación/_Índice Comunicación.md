---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Electrónica y robótica
tags: [indice, electronica]
---
# Comunicación

Parte de: [[63-Electronica-y-Robotica/_Índice Electrónica y robótica|Electrónica y robótica]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[I2C, SPI y UART]]
- [[Wi-Fi y Bluetooth en el ESP32]]
- [[Conectar un ESP32 a un API .NET]] ⭐

## Laboratorios

- [[Lab 4 - ESP32 enviando datos a un API propio]]

## Progreso

```base
filters:
  and:
    - file.inFolder("63-Electronica-y-Robotica/05 Comunicación")
    - file.name != "_Índice Comunicación"
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
