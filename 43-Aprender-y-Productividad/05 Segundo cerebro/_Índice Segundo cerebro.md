---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Aprender a aprender y productividad
tags: [indice, productividad]
---
# Segundo cerebro

Parte de: [[43-Aprender-y-Productividad/_Índice Aprender a aprender y productividad|Aprender a aprender y productividad]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Métodos PARA y CODE]]
- [[Notas atómicas y enlaces]]
- [[Revisión semanal]] ⭐
- [[Obsidian (frontmatter, Dataview y plugins útiles)]] (vive en 10 · Radar)

## Laboratorios

- [[Lab 3 - Mi primera revisión semanal]]

## Progreso

```base
filters:
  and:
    - file.inFolder("43-Aprender-y-Productividad/05 Segundo cerebro")
    - file.name != "_Índice Segundo cerebro"
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
