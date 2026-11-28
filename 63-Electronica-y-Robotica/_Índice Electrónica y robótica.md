---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Electrónica y robótica
tags: [indice, electronica]
contenido: completo
---
# Electrónica y robótica

Avance y prioridades en [[Temario]].

> [!note]
> La sección de electrónica básica del área 37 enlaza aquí.

## Secciones

- [[63-Electronica-y-Robotica/01 Fundamentos/_Índice Fundamentos|Fundamentos]]
- [[63-Electronica-y-Robotica/02 Herramientas/_Índice Herramientas|Herramientas]]
- [[63-Electronica-y-Robotica/03 Microcontroladores/_Índice Microcontroladores|Microcontroladores]]
- [[63-Electronica-y-Robotica/04 Sensores y actuadores/_Índice Sensores y actuadores|Sensores y actuadores]]
- [[63-Electronica-y-Robotica/05 Comunicación/_Índice Comunicación|Comunicación]]
- [[63-Electronica-y-Robotica/06 Energía/_Índice Energía|Energía]]
- [[63-Electronica-y-Robotica/07 Robótica/_Índice Robótica|Robótica]]
- [[63-Electronica-y-Robotica/08 Drones armados por ti/_Índice Drones armados por ti|Drones armados por ti]]
- [[63-Electronica-y-Robotica/09 Diseño de circuitos/_Índice Diseño de circuitos|Diseño de circuitos]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("63-Electronica-y-Robotica")
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
