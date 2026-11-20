---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Impresión 3D
tags: [indice, impresion-3d]
---
# Impresión 3D

Avance y prioridades en [[Temario]]. Mapa visual: [[Mapa mental - Impresión 3D.canvas|Mapa mental]].

## Secciones

- [[36-Impresion-3D/01 Fundamentos/_Índice Fundamentos|Fundamentos]]
- [[36-Impresion-3D/02 Laminador/_Índice Laminador|Laminador]]
- [[36-Impresion-3D/03 Materiales/_Índice Materiales|Materiales]]
- [[36-Impresion-3D/04 Calibración y calidad/_Índice Calibración y calidad|Calibración y calidad]]
- [[36-Impresion-3D/05 Posprocesado/_Índice Posprocesado|Posprocesado]]
- [[36-Impresion-3D/06 Herramientas y compras/_Índice Herramientas y compras|Herramientas y compras]]
- [[36-Impresion-3D/07 Comunidad, licencias y costos/_Índice Comunidad, licencias y costos|Comunidad, licencias y costos]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("36-Impresion-3D")
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
