---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Power BI
tags: [indice, power-bi]
---
# Power BI

Avance y prioridades en [[Temario]]. Mapa visual: [[Mapa mental - Power BI.canvas|Mapa mental]].

## Secciones

- [[17-Power-BI/01 Fundamentos/_Índice Fundamentos|Fundamentos]]
- [[17-Power-BI/02 Power Query y el lenguaje M/_Índice Power Query y el lenguaje M|Power Query y el lenguaje M]]
- [[17-Power-BI/03 Modelado de datos/_Índice Modelado de datos|Modelado de datos]]
- [[17-Power-BI/04 DAX/_Índice DAX|DAX]]
- [[17-Power-BI/05 Ayudas para crear DAX más rápido/_Índice Ayudas para crear DAX más rápido|Ayudas para crear DAX más rápido]]
- [[17-Power-BI/06 Visualización/_Índice Visualización|Visualización]]
- [[17-Power-BI/07 Gráficas propias/_Índice Gráficas propias|Gráficas propias]]
- [[17-Power-BI/08 Power BI Service/_Índice Power BI Service|Power BI Service]]
- [[17-Power-BI/09 Avanzado/_Índice Avanzado|Avanzado]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("17-Power-BI")
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
