---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Aprender a aprender y productividad
tags: [indice, productividad]
---
# Aprender a aprender y productividad

Avance y prioridades en [[Temario]].

## Secciones

- [[43-Aprender-y-Productividad/01 Aprender a aprender/_Índice Aprender a aprender|Aprender a aprender]]
- [[43-Aprender-y-Productividad/02 Gestión del tiempo/_Índice Gestión del tiempo|Gestión del tiempo]]
- [[43-Aprender-y-Productividad/03 Enfoque/_Índice Enfoque|Enfoque]]
- [[43-Aprender-y-Productividad/04 Hábitos/_Índice Hábitos|Hábitos]]
- [[43-Aprender-y-Productividad/05 Segundo cerebro/_Índice Segundo cerebro|Segundo cerebro]]
- [[43-Aprender-y-Productividad/06 Energía y burnout/_Índice Energía y burnout|Energía y burnout]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("43-Aprender-y-Productividad")
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
