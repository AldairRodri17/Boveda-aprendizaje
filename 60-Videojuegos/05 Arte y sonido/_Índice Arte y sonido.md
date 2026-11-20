---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Desarrollo de videojuegos
tags: [indice, videojuegos]
---
# Arte y sonido

Parte de: [[60-Videojuegos/_Índice Desarrollo de videojuegos|Desarrollo de videojuegos]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Sprites y pixel art]]
- [[Usar modelos de Blender en un juego]] (enlaza con 38)
- [[Animación de personajes]]
- [[Sonido y música para videojuegos]]
- [[Assets gratuitos y sus licencias]]

## Laboratorios

- [[Lab 4 - Juego 3D con un modelo propio de Blender]]

## Progreso

```base
filters:
  and:
    - file.inFolder("60-Videojuegos/05 Arte y sonido")
    - file.name != "_Índice Arte y sonido"
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
