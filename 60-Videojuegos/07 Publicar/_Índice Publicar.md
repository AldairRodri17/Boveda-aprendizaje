---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Desarrollo de videojuegos
tags: [indice, videojuegos]
contenido: completo
---
# Publicar

Parte de: [[60-Videojuegos/_Índice Desarrollo de videojuegos|Desarrollo de videojuegos]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Game jams]] ⭐
- [[Publicar en itch.io]]
- [[Publicar en Steam (conceptos)]]
- [[Pruebas con jugadores (playtesting)]]

## Laboratorios

- [[Lab 3 - Participar en una game jam]]

## Progreso

```base
filters:
  and:
    - file.inFolder("60-Videojuegos/07 Publicar")
    - file.name != "_Índice Publicar"
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
