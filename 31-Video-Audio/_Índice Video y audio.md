---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Video y audio
tags: [indice, video-audio]
---
# Video y audio

Avance y prioridades en [[Temario]]. Mapa visual: [[Mapa mental - Video y audio.canvas|Mapa mental]].

## Secciones

- [[31-Video-Audio/01 Técnica de video/_Índice Técnica de video|Técnica de video]]
- [[31-Video-Audio/02 Lenguaje audiovisual/_Índice Lenguaje audiovisual|Lenguaje audiovisual]]
- [[31-Video-Audio/03 Audio/_Índice Audio|Audio]]
- [[31-Video-Audio/04 Producción/_Índice Producción|Producción]]
- [[31-Video-Audio/05 Formatos/_Índice Formatos|Formatos]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("31-Video-Audio")
    - tipo != "indice"
views:
  - type: table
    name: Pendientes
    filters:
      and:
        - progreso != "visto"
        - progreso != "dominado"
    order:
      - file.name
      - file.folder
      - progreso
      - nivel
  - type: table
    name: Vistos
    filters:
      and:
        - progreso == "visto"
    order:
      - file.name
      - file.folder
  - type: table
    name: Dominados
    filters:
      and:
        - progreso == "dominado"
    order:
      - file.name
      - file.folder
```
