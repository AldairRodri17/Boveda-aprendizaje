---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Video y audio
tags: [indice, video-audio]
contenido: completo
---
# Técnica de video

Parte de: [[31-Video-Audio/_Índice Video y audio|Video y audio]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Resolución y cuadros por segundo]]
- [[Regla de los 180 grados del obturador]]
- [[Cámara lenta (high frame rate)]]
- [[Códecs y contenedores de video (H.264, H.265 y ProRes)]]
- [[Bitrate y tamaño de archivo]]
- [[Profundidad de bits y submuestreo de croma]]
- [[Perfiles de color en video (estándar, Log y HLG)]]
- [[Herramientas de exposición en video (zebras, waveform y false color)]]
- [[Filtros ND en video]]

## Laboratorios

- [[Lab 1 - Obturación a 180 grados y cámara lenta]]
- [[Lab 2 - Exposición de Apple Log con zebras y false color]]

## Progreso

```base
filters:
  and:
    - file.inFolder("31-Video-Audio/01 Técnica de video")
    - file.name != "_Índice Técnica de video"
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
