---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: JavaScript y Web
tags: [indice, javascript-web]
contenido: completo
---
# JavaScript y Web

Avance y prioridades en [[Temario]]. Mapa visual: [[Mapa mental - JavaScript y Web.canvas|Mapa mental]].

## Secciones

- [[04-JavaScript-Web/01 HTML y CSS/_Índice HTML y CSS|HTML y CSS]]
- [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]
- [[04-JavaScript-Web/03 Asincronía en JS/_Índice Asincronía en JS|Asincronía en JS]]
- [[04-JavaScript-Web/04 El navegador/_Índice El navegador|El navegador]]
- [[04-JavaScript-Web/05 Front sin bundler/_Índice Front sin bundler|Front sin bundler]]
- [[04-JavaScript-Web/06 Avanzado/_Índice Avanzado|Avanzado]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("04-JavaScript-Web")
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
