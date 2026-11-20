---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Gestión de personas
tags: [indice, gestion-personas]
---
# Reuniones uno a uno

Parte de: [[53-Gestion-de-Personas/_Índice Gestión de personas|Gestión de personas]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Reuniones 1:1]] ⭐
- [[Preguntas para reuniones 1:1]]

## Laboratorios

- [[Lab 1 - Simular un 1:1 con Claude]]

## Progreso

```base
filters:
  and:
    - file.inFolder("53-Gestion-de-Personas/02 Reuniones uno a uno")
    - file.name != "_Índice Reuniones uno a uno"
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
