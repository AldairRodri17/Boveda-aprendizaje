---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Aprender a aprender y productividad
tags: [indice, productividad]
---
# Energía y burnout

Parte de: [[43-Aprender-y-Productividad/_Índice Aprender a aprender y productividad|Aprender a aprender y productividad]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Señales de burnout]] ⭐
- [[Descanso y recuperación]]
- [[Balance de tiempo entre carrera y hobbies]] (vive en 39)

## Progreso

```base
filters:
  and:
    - file.inFolder("43-Aprender-y-Productividad/06 Energía y burnout")
    - file.name != "_Índice Energía y burnout"
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
