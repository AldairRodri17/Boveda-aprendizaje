---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Freelance y emprendimiento
tags: [indice, freelance]
---
# Legal y fiscal

Parte de: [[45-Freelance-y-Emprendimiento/_Índice Freelance y emprendimiento|Freelance y emprendimiento]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Alta en el SAT para trabajar por tu cuenta]]
- [[RESICO para personas físicas]] ⭐
- [[Facturar como freelance]]
- [[Contratos y propiedad intelectual]] (enlaza con 48)

## Progreso

```base
filters:
  and:
    - file.inFolder("45-Freelance-y-Emprendimiento/04 Legal y fiscal")
    - file.name != "_Índice Legal y fiscal"
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
