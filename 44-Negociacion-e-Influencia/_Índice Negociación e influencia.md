---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Negociación e influencia
tags: [indice, negociacion]
---
# Negociación e influencia

Avance y prioridades en [[Temario]].

## Secciones

- [[44-Negociacion-e-Influencia/01 Negociación/_Índice Negociación|Negociación]]
- [[44-Negociacion-e-Influencia/02 Inteligencia emocional/_Índice Inteligencia emocional|Inteligencia emocional]]
- [[44-Negociacion-e-Influencia/03 Comunicación asertiva/_Índice Comunicación asertiva|Comunicación asertiva]]
- [[44-Negociacion-e-Influencia/04 Persuasión/_Índice Persuasión|Persuasión]]
- [[44-Negociacion-e-Influencia/05 Networking y marca personal/_Índice Networking y marca personal|Networking y marca personal]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("44-Negociacion-e-Influencia")
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
