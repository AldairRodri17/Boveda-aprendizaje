---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Edición de video
tags: [indice, edicion-video]
contenido: completo
---
# Edición de video

Avance y prioridades en [[Temario]]. Mapa visual: [[Mapa mental - Edición de video.canvas|Mapa mental]].

## Secciones

- [[33-Edicion-Video/01 Fundamentos/_Índice Fundamentos|Fundamentos]]
- [[33-Edicion-Video/02 Montaje/_Índice Montaje|Montaje]]
- [[33-Edicion-Video/03 Audio en posproducción/_Índice Audio en posproducción|Audio en posproducción]]
- [[33-Edicion-Video/04 Color/_Índice Color|Color]]
- [[33-Edicion-Video/05 Gráficos y efectos/_Índice Gráficos y efectos|Gráficos y efectos]]
- [[33-Edicion-Video/06 Entrega/_Índice Entrega|Entrega]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("33-Edicion-Video")
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
