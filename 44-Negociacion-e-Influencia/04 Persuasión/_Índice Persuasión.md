---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Negociación e influencia
tags: [indice, negociacion]
---
# Persuasión

Parte de: [[44-Negociacion-e-Influencia/_Índice Negociación e influencia|Negociación e influencia]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Principios de persuasión con ética]]
- [[Argumentar con datos e historias]]

## Progreso

```base
filters:
  and:
    - file.inFolder("44-Negociacion-e-Influencia/04 Persuasión")
    - file.name != "_Índice Persuasión"
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
