---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Equipo
tags: [indice, equipo]
contenido: completo
---
# Equipo

Avance y prioridades en [[Temario]]. Mapa visual: [[Mapa mental - Equipo.canvas|Mapa mental]].

## Secciones

- [[34-Equipo/01 Mi equipo actual/_Índice Mi equipo actual|Mi equipo actual]]
- [[34-Equipo/02 Próximo equipo/_Índice Próximo equipo|Próximo equipo]]
- [[34-Equipo/03 Compras y accesorios/_Índice Compras y accesorios|Compras y accesorios]]
- [[34-Equipo/04 Certificaciones opcionales/_Índice Certificaciones opcionales|Certificaciones opcionales]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("34-Equipo")
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
