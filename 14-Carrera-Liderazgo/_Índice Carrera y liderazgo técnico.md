---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Carrera y liderazgo técnico
tags: [indice, carrera-liderazgo]
---
# Carrera y liderazgo técnico

Avance y prioridades en [[Temario]]. Mapa visual: [[Mapa mental - Carrera y liderazgo técnico.canvas|Mapa mental]].

## Secciones

- [[14-Carrera-Liderazgo/01 Qué es ser senior/_Índice Qué es ser senior|Qué es ser senior]]
- [[14-Carrera-Liderazgo/02 Liderazgo y comunicación/_Índice Liderazgo y comunicación|Liderazgo y comunicación]]
- [[14-Carrera-Liderazgo/03 Proceso de trabajo/_Índice Proceso de trabajo|Proceso de trabajo]]
- [[14-Carrera-Liderazgo/04 Búsqueda de trabajo/_Índice Búsqueda de trabajo|Búsqueda de trabajo]]
- [[14-Carrera-Liderazgo/05 Inglés técnico/_Índice Inglés técnico|Inglés técnico]]
- [[14-Carrera-Liderazgo/06 Hábitos/_Índice Hábitos|Hábitos]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("14-Carrera-Liderazgo")
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
