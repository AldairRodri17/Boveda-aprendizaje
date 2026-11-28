---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Drones
tags: [indice, drones]
contenido: completo
---
# Foto y video aéreo

Parte de: [[35-Drones/_Índice Drones|Drones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Composición aérea y cenital]]
- [[Panorámicas y HDR con dron]]
- [[Hyperlapse con dron]]
- [[Exposición en video aéreo (ND y 180 grados)]]
- [[Movimientos cinematográficos con dron]]

## Laboratorios

- [[Lab 3 - Reveal cinematográfico de 15 segundos]]
- [[Lab 4 - Panorámica y hyperlapse con el Mini 4K]]

## Progreso

```base
filters:
  and:
    - file.inFolder("35-Drones/03 Foto y video aéreo")
    - file.name != "_Índice Foto y video aéreo"
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
