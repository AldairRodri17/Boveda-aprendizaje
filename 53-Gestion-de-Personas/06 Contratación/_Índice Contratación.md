---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Gestión de personas
tags: [indice, gestion-personas]
---
# Contratación

Parte de: [[53-Gestion-de-Personas/_Índice Gestión de personas|Gestión de personas]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Definir el perfil del puesto]]
- [[Entrevistar candidatos (ser el entrevistador)]] (vive en 14 · Carrera)
- [[Decidir y dar respuesta a candidatos]]

## Progreso

```base
filters:
  and:
    - file.inFolder("53-Gestion-de-Personas/06 Contratación")
    - file.name != "_Índice Contratación"
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
