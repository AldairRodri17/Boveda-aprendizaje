---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Desarrollo de videojuegos
tags: [indice, videojuegos]
contenido: completo
---
# Fundamentos

Parte de: [[60-Videojuegos/_Índice Desarrollo de videojuegos|Desarrollo de videojuegos]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Qué es un motor de videojuegos]] ⭐
- [[Godot vs Unity vs Unreal]] ⭐
- [[El game loop]] ⭐
- [[Escenas, nodos y entidades]]
- [[Matemáticas para videojuegos (vectores y trigonometría)]]

## Progreso

```base
filters:
  and:
    - file.inFolder("60-Videojuegos/01 Fundamentos")
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
