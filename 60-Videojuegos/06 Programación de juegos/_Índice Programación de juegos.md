---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Desarrollo de videojuegos
tags: [indice, videojuegos]
---
# Programación de juegos

Parte de: [[60-Videojuegos/_Índice Desarrollo de videojuegos|Desarrollo de videojuegos]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Máquinas de estado para personajes]] ⭐
- [[Patrones de diseño en videojuegos (componentes, observer y object pool)]]
- [[Colisiones]]
- [[Inteligencia artificial básica en juegos (pathfinding A*)]]
- [[Guardar partidas]]
- [[Interfaz de usuario en videojuegos]]

## Laboratorios

- [[Lab 2 - Plataformero 2D con C#]]

## Progreso

```base
filters:
  and:
    - file.inFolder("60-Videojuegos/06 Programación de juegos")
    - file.name != "_Índice Programación de juegos"
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
