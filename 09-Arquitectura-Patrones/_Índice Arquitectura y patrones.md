---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Arquitectura y patrones
tags: [indice, arquitectura-patrones]
contenido: completo
---
# Arquitectura y patrones

Avance y prioridades en [[Temario]]. Mapa visual: [[Mapa mental - Arquitectura y patrones.canvas|Mapa mental]].

## Secciones

- [[09-Arquitectura-Patrones/01 Estilos de arquitectura/_Índice Estilos de arquitectura|Estilos de arquitectura]]
- [[09-Arquitectura-Patrones/02 Patrones de diseño/_Índice Patrones de diseño|Patrones de diseño]]
- [[09-Arquitectura-Patrones/03 Patrones de nube y sistemas distribuidos/_Índice Patrones de nube y sistemas distribuidos|Patrones de nube y sistemas distribuidos]]
- [[09-Arquitectura-Patrones/04 Mensajería/_Índice Mensajería|Mensajería]]
- [[09-Arquitectura-Patrones/05 Diseño de sistemas/_Índice Diseño de sistemas|Diseño de sistemas]]
- [[09-Arquitectura-Patrones/06 Documentar decisiones/_Índice Documentar decisiones|Documentar decisiones]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("09-Arquitectura-Patrones")
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
