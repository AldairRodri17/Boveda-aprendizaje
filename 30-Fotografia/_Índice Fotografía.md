---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Fotografía
tags: [indice, fotografia]
---
# Fotografía

Avance y prioridades en [[Temario]]. Mapa visual: [[Mapa mental - Fotografía.canvas|Mapa mental]].

## Secciones

- [[30-Fotografia/01 Fundamentos de la cámara/_Índice Fundamentos de la cámara|Fundamentos de la cámara]]
- [[30-Fotografia/02 Exposición/_Índice Exposición|Exposición]]
- [[30-Fotografia/03 Enfoque/_Índice Enfoque|Enfoque]]
- [[30-Fotografia/04 Luz y color/_Índice Luz y color|Luz y color]]
- [[30-Fotografia/05 Composición/_Índice Composición|Composición]]
- [[30-Fotografia/06 Géneros/_Índice Géneros|Géneros]]
- [[30-Fotografia/07 Avanzado/_Índice Avanzado|Avanzado]]
- [[30-Fotografia/08 Flujo, publicación y legal/_Índice Flujo, publicación y legal|Flujo, publicación y legal]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("30-Fotografia")
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
