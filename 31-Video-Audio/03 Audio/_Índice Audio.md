---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Video y audio
tags: [indice, video-audio]
---
# Audio

Parte de: [[31-Video-Audio/_Índice Video y audio|Video y audio]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Tipos de micrófono (lavalier, shotgun y de cámara)]]
- [[Ganancia y niveles de audio]]
- [[Colocación del micrófono]]
- [[Sonido ambiente y voz en off]]
- [[Grabar entrevistas a dos personas]]
- [[Sincronizar audio y video]]

## Laboratorios

- [[Lab 5 - Prueba de audio con el micrófono del iPhone y el DJI Mic Mini 2]]
- [[Lab 6 - Entrevista a dos personas]]

## Progreso

```base
filters:
  and:
    - file.inFolder("31-Video-Audio/03 Audio")
    - file.name != "_Índice Audio"
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
