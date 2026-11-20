---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Modelado 3D artístico
tags: [indice, modelado-3d-artistico]
---
# Modelado 3D artístico

Avance y prioridades en [[Temario]]. Mapa visual: [[Mapa mental - Modelado 3D artístico.canvas|Mapa mental]].

## Secciones

- [[38-Modelado-3D-Artistico/01 Blender/_Índice Blender|Blender]]
- [[38-Modelado-3D-Artistico/02 Otras herramientas/_Índice Otras herramientas|Otras herramientas]]
- [[38-Modelado-3D-Artistico/03 Técnicas artísticas/_Índice Técnicas artísticas|Técnicas artísticas]]
- [[38-Modelado-3D-Artistico/04 Escaneo 3D/_Índice Escaneo 3D|Escaneo 3D]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("38-Modelado-3D-Artistico")
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
