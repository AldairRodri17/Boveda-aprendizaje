---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Gestión de personas
tags: [indice, gestion-personas]
---
# Gestión de personas

Avance y prioridades en [[Temario]].

> [!note]
> Para cuando tengas gente a cargo. Complementa 14 · Carrera (influir sin autoridad).

## Secciones

- [[53-Gestion-de-Personas/01 El rol de líder/_Índice El rol de líder|El rol de líder]]
- [[53-Gestion-de-Personas/02 Reuniones uno a uno/_Índice Reuniones uno a uno|Reuniones uno a uno]]
- [[53-Gestion-de-Personas/03 Delegar y desarrollar/_Índice Delegar y desarrollar|Delegar y desarrollar]]
- [[53-Gestion-de-Personas/04 Desempeño/_Índice Desempeño|Desempeño]]
- [[53-Gestion-de-Personas/05 Equipo/_Índice Equipo|Equipo]]
- [[53-Gestion-de-Personas/06 Contratación/_Índice Contratación|Contratación]]
- [[53-Gestion-de-Personas/07 Gestión hacia arriba/_Índice Gestión hacia arriba|Gestión hacia arriba]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("53-Gestion-de-Personas")
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
