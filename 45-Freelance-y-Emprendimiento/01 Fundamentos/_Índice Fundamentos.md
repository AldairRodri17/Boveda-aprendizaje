---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Freelance y emprendimiento
tags: [indice, freelance]
---
# Fundamentos

Parte de: [[45-Freelance-y-Emprendimiento/_Índice Freelance y emprendimiento|Freelance y emprendimiento]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Freelance vs empleo vs empresa propia]] ⭐
- [[Validar una idea de negocio]] ⭐
- [[Modelo de negocio en una página (Lean Canvas)]]

## Progreso

```base
filters:
  and:
    - file.inFolder("45-Freelance-y-Emprendimiento/01 Fundamentos")
    - file.name != "_Índice Fundamentos"
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
