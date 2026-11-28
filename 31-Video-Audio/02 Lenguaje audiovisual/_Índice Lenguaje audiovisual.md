---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Video y audio
tags: [indice, video-audio]
contenido: completo
---
# Lenguaje audiovisual

Parte de: [[31-Video-Audio/_Índice Video y audio|Video y audio]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Tipos de plano]]
  - [[Plano general (video)]]
  - [[Plano medio (video)]]
  - [[Primer plano (video)]]
  - [[Plano detalle (video)]]
- [[Ángulos de cámara]]
- [[Movimientos de cámara]]
  - [[Paneo (movimiento de cámara)]]
  - [[Tilt (movimiento de cámara)]]
  - [[Travelling (movimiento de cámara)]]
  - [[Dolly in y dolly out]]
  - [[Cámara en mano (movimiento de cámara)]]
- [[Eje de 180 grados y continuidad]]
- [[B-roll y plano de ubicación]]
- [[Narrativa y storytelling en video]]

## Laboratorios

- [[Lab 3 - Escena de 60 segundos con 5 tipos de plano sin romper el eje]]
- [[Lab 4 - Movimientos de cámara con el iPhone]]

## Progreso

```base
filters:
  and:
    - file.inFolder("31-Video-Audio/02 Lenguaje audiovisual")
    - file.name != "_Índice Lenguaje audiovisual"
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
