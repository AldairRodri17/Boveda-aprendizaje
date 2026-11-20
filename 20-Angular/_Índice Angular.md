---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Angular
tags: [indice, angular]
---
# Angular

Avance y prioridades en [[Temario]]. Mapa visual: [[Mapa mental - Angular.canvas|Mapa mental]].

## Secciones

- [[20-Angular/01 Básico/_Índice Básico|Básico]]
- [[20-Angular/02 Intermedio/_Índice Intermedio|Intermedio]]
- [[20-Angular/03 Avanzado/_Índice Avanzado|Avanzado]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("20-Angular")
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
