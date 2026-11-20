---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Drones
tags: [indice, drones]
---
# Drones

Avance y prioridades en [[Temario]]. Mapa visual: [[Mapa mental - Drones.canvas|Mapa mental]].

## Secciones

- [[35-Drones/01 Normativa y seguridad/_Índice Normativa y seguridad|Normativa y seguridad]]
- [[35-Drones/02 Pilotaje/_Índice Pilotaje|Pilotaje]]
- [[35-Drones/03 Foto y video aéreo/_Índice Foto y video aéreo|Foto y video aéreo]]
- [[35-Drones/04 Edición y mantenimiento/_Índice Edición y mantenimiento|Edición y mantenimiento]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("35-Drones")
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
