---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Astronomía y astrofotografía
tags: [indice, astronomia]
---
# Astrofotografía básica

Parte de: [[64-Astronomia/_Índice Astronomía y astrofotografía|Astronomía y astrofotografía]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Vía Láctea con cámara fija]] ⭐
- [[Regla de los 500 y regla NPF]] ⭐
- [[Enfocar las estrellas]]
- [[Astrofotografía con iPhone]]
- [[Astrofotografía con la Sony a6700]]
- [[Star trails (rastros de estrellas)]]
- [[Timelapse nocturno]]

## Laboratorios

- [[Lab 2 - Mi primera Vía Láctea]]
- [[Lab 4 - Star trails]]

## Progreso

```base
filters:
  and:
    - file.inFolder("64-Astronomia/03 Astrofotografía básica")
    - file.name != "_Índice Astrofotografía básica"
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
