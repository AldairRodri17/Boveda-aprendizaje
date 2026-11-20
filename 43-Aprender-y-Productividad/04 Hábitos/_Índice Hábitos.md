---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Aprender a aprender y productividad
tags: [indice, productividad]
---
# Hábitos

Parte de: [[43-Aprender-y-Productividad/_Índice Aprender a aprender y productividad|Aprender a aprender y productividad]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Cómo se forma un hábito]] ⭐
- [[Rutinas de mañana y noche]]
- [[Seguimiento de hábitos]]

## Progreso

```base
filters:
  and:
    - file.inFolder("43-Aprender-y-Productividad/04 Hábitos")
    - file.name != "_Índice Hábitos"
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
