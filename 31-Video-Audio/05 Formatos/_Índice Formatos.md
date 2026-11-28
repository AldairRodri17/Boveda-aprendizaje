---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Video y audio
tags: [indice, video-audio]
contenido: completo
---
# Formatos

Parte de: [[31-Video-Audio/_Índice Video y audio|Video y audio]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Video vertical (Reels, TikTok y Shorts)]]
- [[Video para YouTube]]
- [[Entrevista en video]]
- [[Vlog]]
- [[Tutorial y screencast]]
- [[Video de producto]]
- [[Cortometraje]]
- [[Time-lapse e hyperlapse en video]]
- [[Creación de contenido (ganchos, retención y constancia)]]

## Laboratorios

- [[Lab 9 - Grabar un reel vertical de 30 segundos]]

## Progreso

```base
filters:
  and:
    - file.inFolder("31-Video-Audio/05 Formatos")
    - file.name != "_Índice Formatos"
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
