---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Astronomía y astrofotografía
tags: [indice, astronomia]
contenido: completo
---
# Astronomía y astrofotografía

Avance y prioridades en [[Temario]].

> [!note]
> Se apoya en 30 · Fotografía, 32 · Edición de foto y tu equipo (34).

## Secciones

- [[64-Astronomia/01 Fundamentos/_Índice Fundamentos|Fundamentos]]
- [[64-Astronomia/02 Observación/_Índice Observación|Observación]]
- [[64-Astronomia/03 Astrofotografía básica/_Índice Astrofotografía básica|Astrofotografía básica]]
- [[64-Astronomia/04 Astrofotografía avanzada/_Índice Astrofotografía avanzada|Astrofotografía avanzada]]
- [[64-Astronomia/05 Edición/_Índice Edición|Edición]]
- [[64-Astronomia/06 Planeación/_Índice Planeación|Planeación]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("64-Astronomia")
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
