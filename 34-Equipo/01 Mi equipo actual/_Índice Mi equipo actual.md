---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Equipo
tags: [indice, equipo]
---
# Mi equipo actual

Parte de: [[34-Equipo/_Índice Equipo|Equipo]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[iPhone 15 Pro para foto y video]]
  - [[Lentes y zoom del iPhone 15 Pro]]
  - [[Apple ProRAW en el iPhone 15 Pro]]
  - [[ProRes y Apple Log en el iPhone 15 Pro]]
- [[Blackmagic Camera (app)]]
- [[DJI Mic Mini 2]]
- [[DJI Mini 4K]]

## Laboratorios

- [[Lab 1 - Conoce tu iPhone 15 Pro (lentes, ProRAW y Blackmagic Camera a 180 grados)]]
- [[Lab 2 - Audio limpio con el DJI Mic Mini 2 en el iPhone]]

## Progreso

```base
filters:
  and:
    - file.inFolder("34-Equipo/01 Mi equipo actual")
    - file.name != "_Índice Mi equipo actual"
views:
  - type: table
    name: Temas
    filters:
      and:
        - tipo != "laboratorio"
    order:
      - file.name
      - progreso
      - nivel
  - type: table
    name: Laboratorios
    filters:
      and:
        - tipo == "laboratorio"
    order:
      - file.name
      - progreso
```
