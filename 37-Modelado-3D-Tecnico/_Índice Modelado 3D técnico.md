---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Modelado 3D técnico
tags: [indice, modelado-3d-tecnico]
contenido: completo
---
# Modelado 3D técnico

Avance y prioridades en [[Temario]]. Mapa visual: [[Mapa mental - Modelado 3D técnico.canvas|Mapa mental]].

## Secciones

- [[37-Modelado-3D-Tecnico/01 Programas gratuitos/_Índice Programas gratuitos|Programas gratuitos]]
- [[37-Modelado-3D-Tecnico/02 Modelado paramétrico/_Índice Modelado paramétrico|Modelado paramétrico]]
- [[37-Modelado-3D-Tecnico/03 Diseño para impresión 3D/_Índice Diseño para impresión 3D|Diseño para impresión 3D]]
- [[37-Modelado-3D-Tecnico/04 Proyectos técnicos/_Índice Proyectos técnicos|Proyectos técnicos]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("37-Modelado-3D-Tecnico")
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
