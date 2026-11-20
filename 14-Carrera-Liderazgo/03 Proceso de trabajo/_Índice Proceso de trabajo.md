---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Carrera y liderazgo técnico
tags: [indice, carrera-liderazgo]
---
# Proceso de trabajo

Parte de: [[14-Carrera-Liderazgo/_Índice Carrera y liderazgo técnico|Carrera y liderazgo técnico]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Scrum en la práctica]]
- [[Kanban]]
- [[Historias de usuario y criterios de aceptación]]
- [[Definition of done]]
- [[Levantamiento de requisitos]]

## Progreso

```base
filters:
  and:
    - file.inFolder("14-Carrera-Liderazgo/03 Proceso de trabajo")
    - file.name != "_Índice Proceso de trabajo"
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
