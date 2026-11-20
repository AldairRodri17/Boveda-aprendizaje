---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Video y audio
tags: [indice, video-audio]
---
# Producción

Parte de: [[31-Video-Audio/_Índice Video y audio|Video y audio]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Iluminación de tres puntos]]
- [[Luz LED continua]]
- [[Estabilización (tripié, gimbal y estabilizador de cámara)]]
- [[Preproducción (idea, guion y guion técnico)]]
- [[Storyboard y lista de tomas]]
- [[Plan de rodaje]]

## Laboratorios

- [[Lab 7 - Iluminación de tres puntos con luz de casa]]
- [[Lab 8 - Preproducción de un video de 2 minutos]]

## Progreso

```base
filters:
  and:
    - file.inFolder("31-Video-Audio/04 Producción")
    - file.name != "_Índice Producción"
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
