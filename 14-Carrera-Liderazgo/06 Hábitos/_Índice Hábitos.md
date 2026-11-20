---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Carrera y liderazgo técnico
tags: [indice, carrera-liderazgo]
---
# Hábitos

Parte de: [[14-Carrera-Liderazgo/_Índice Carrera y liderazgo técnico|Carrera y liderazgo técnico]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Diario de aprendizaje]]
- [[Práctica deliberada (katas de código y de diseño)]]
- [[Leer código de otros]]
- [[Contribuir a open source]]

## Progreso

```base
filters:
  and:
    - file.inFolder("14-Carrera-Liderazgo/06 Hábitos")
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
