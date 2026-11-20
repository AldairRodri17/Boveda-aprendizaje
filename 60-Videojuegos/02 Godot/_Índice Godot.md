---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Desarrollo de videojuegos
tags: [indice, videojuegos]
---
# Godot

Parte de: [[60-Videojuegos/_Índice Desarrollo de videojuegos|Desarrollo de videojuegos]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Godot - interfaz y escenas]] ⭐
- [[GDScript básico]]
- [[C# en Godot]] ⭐
- [[Señales en Godot]]
- [[Física 2D en Godot]]
- [[Exportar un juego de Godot]]

## Laboratorios

- [[Lab 1 - Pong en Godot]]

## Progreso

```base
filters:
  and:
    - file.inFolder("60-Videojuegos/02 Godot")
    - file.name != "_Índice Godot"
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
