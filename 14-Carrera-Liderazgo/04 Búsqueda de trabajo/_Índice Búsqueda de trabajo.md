---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Carrera y liderazgo técnico
tags: [indice, carrera-liderazgo]
---
# Búsqueda de trabajo

Parte de: [[14-Carrera-Liderazgo/_Índice Carrera y liderazgo técnico|Carrera y liderazgo técnico]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Portafolio y GitHub]]
- [[Preparación para entrevistas técnicas]]
- [[Entrevistas de comportamiento (método STAR)]]
- [[Entrevistas de diseño de sistemas]]
- [[Negociación salarial]]

## Laboratorios

- [[Lab 3 - Preparar historias STAR]]
- [[Lab 4 - Simulacro de entrevista de diseño de sistemas]]

## Progreso

```base
filters:
  and:
    - file.inFolder("14-Carrera-Liderazgo/04 Búsqueda de trabajo")
    - file.name != "_Índice Búsqueda de trabajo"
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
