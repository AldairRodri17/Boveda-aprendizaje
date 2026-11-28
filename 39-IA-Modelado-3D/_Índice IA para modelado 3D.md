---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: IA para modelado 3D
tags: [indice, ia-modelado-3d]
contenido: completo
---
# IA para modelado 3D

Avance y prioridades en [[Temario]]. Mapa visual: [[Mapa mental - IA para modelado 3D.canvas|Mapa mental]].

## Secciones

- [[39-IA-Modelado-3D/01 Conceptos/_Índice Conceptos|Conceptos]]
- [[39-IA-Modelado-3D/02 Entorno local/_Índice Entorno local|Entorno local]]
- [[39-IA-Modelado-3D/03 Modelos/_Índice Modelos|Modelos]]
- [[39-IA-Modelado-3D/04 Flujo completo/_Índice Flujo completo|Flujo completo]]
- [[39-IA-Modelado-3D/05 Calidad y criterio/_Índice Calidad y criterio|Calidad y criterio]]
- [[39-IA-Modelado-3D/06 Licencias y ética/_Índice Licencias y ética|Licencias y ética]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("39-IA-Modelado-3D")
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
