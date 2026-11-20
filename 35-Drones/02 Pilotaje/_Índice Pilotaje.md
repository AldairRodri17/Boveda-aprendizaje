---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Drones
tags: [indice, drones]
---
# Pilotaje

Parte de: [[35-Drones/_Índice Drones|Drones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Controles y modos de vuelo (Cine, Normal y Sport)]]
- [[GPS, calibración y sensores del dron]]
- [[Simuladores de vuelo de drones]]
- [[Maniobras básicas con dron]]
  - [[Despegue, vuelo estacionario y aterrizaje con dron]]
  - [[Vuelo en cuadrado con dron]]
  - [[Vuelo lineal de ida y vuelta con dron]]
  - [[Órbita alrededor de un punto con dron]]
  - [[Ocho horizontal con dron]]
  - [[Ascenso con inclinación de cámara con dron]]
- [[QuickShots y modos automáticos]]

## Laboratorios

- [[Lab 2 - Seis maniobras básicas en simulador y en campo abierto]]

## Progreso

```base
filters:
  and:
    - file.inFolder("35-Drones/02 Pilotaje")
    - file.name != "_Índice Pilotaje"
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
