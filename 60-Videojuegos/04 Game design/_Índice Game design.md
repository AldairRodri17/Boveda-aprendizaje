---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Desarrollo de videojuegos
tags: [indice, videojuegos]
---
# Game design

Parte de: [[60-Videojuegos/_Índice Desarrollo de videojuegos|Desarrollo de videojuegos]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Mecánicas, dinámicas y estética (MDA)]] ⭐
- [[Diseño de niveles]]
- [[Curva de dificultad]]
- [[Game feel (que el juego se sienta bien)]]
- [[Prototipar rápido un juego]] ⭐

## Progreso

```base
filters:
  and:
    - file.inFolder("60-Videojuegos/04 Game design")
    - file.name != "_Índice Game design"
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
