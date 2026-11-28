---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Homelab y self-hosting
tags: [indice, homelab]
---
# Homelab y self-hosting

Avance y prioridades en [[Temario]].

> [!note]
> Tu propio servidor en casa: practicas Linux, redes, Docker y respaldos sin pagar nube.

## Secciones

- [[61-Homelab/01 Fundamentos/_Índice Fundamentos|Fundamentos]]
- [[61-Homelab/02 Sistema y virtualización/_Índice Sistema y virtualización|Sistema y virtualización]]
- [[61-Homelab/03 Almacenamiento/_Índice Almacenamiento|Almacenamiento]]
- [[61-Homelab/04 Red del homelab/_Índice Red del homelab|Red del homelab]]
- [[61-Homelab/05 Acceso remoto/_Índice Acceso remoto|Acceso remoto]]
- [[61-Homelab/06 Servicios/_Índice Servicios|Servicios]]
- [[61-Homelab/07 Respaldo y mantenimiento/_Índice Respaldo y mantenimiento|Respaldo y mantenimiento]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("61-Homelab")
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
