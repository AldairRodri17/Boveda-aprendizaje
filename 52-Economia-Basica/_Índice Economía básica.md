---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Economía básica
tags: [indice, economia]
contenido: completo
---
# Economía básica

Avance y prioridades en [[Temario]].

> [!note]
> Datos de inflación, tasas y tipo de cambio se consultan en Banxico e INEGI al escribir cada nota.

## Secciones

- [[52-Economia-Basica/01 Conceptos/_Índice Conceptos|Conceptos]]
- [[52-Economia-Basica/02 Inflación y tasas/_Índice Inflación y tasas|Inflación y tasas]]
- [[52-Economia-Basica/03 Tipo de cambio y comercio/_Índice Tipo de cambio y comercio|Tipo de cambio y comercio]]
- [[52-Economia-Basica/04 Ciclos y mercados/_Índice Ciclos y mercados|Ciclos y mercados]]
- [[52-Economia-Basica/05 Leer noticias económicas/_Índice Leer noticias económicas|Leer noticias económicas]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("52-Economia-Basica")
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
