---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Fundamentos
tags: [indice, fundamentos]
---
# Fundamentos

Avance y prioridades en [[Temario]]. Mapa visual: [[Mapa mental - Fundamentos.canvas|Mapa mental]].

## Secciones

- [[02-Fundamentos/01 Internet y web/_Índice Internet y web|Internet y web]]
- [[02-Fundamentos/02 HTTP/_Índice HTTP|HTTP]]
- [[02-Fundamentos/03 APIs/_Índice APIs|APIs]]
- [[02-Fundamentos/04 Cookies/_Índice Cookies|Cookies]]
- [[02-Fundamentos/05 Autenticación y autorización/_Índice Autenticación y autorización|Autenticación y autorización]]
- [[02-Fundamentos/06 Datos y formatos/_Índice Datos y formatos|Datos y formatos]]
- [[02-Fundamentos/07 Git/_Índice Git|Git]]
- [[02-Fundamentos/08 Computación general/_Índice Computación general|Computación general]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("02-Fundamentos")
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
