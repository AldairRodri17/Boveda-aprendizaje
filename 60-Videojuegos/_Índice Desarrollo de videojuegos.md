---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Desarrollo de videojuegos
tags: [indice, videojuegos]
contenido: completo
---
# Desarrollo de videojuegos

Avance y prioridades en [[Temario]].

> [!note]
> Unity y Godot usan C#: aprovecha lo que sabes de .NET. Licencias y precios de motores se verifican al escribir cada nota.

## Secciones

- [[60-Videojuegos/01 Fundamentos/_Índice Fundamentos|Fundamentos]]
- [[60-Videojuegos/02 Godot/_Índice Godot|Godot]]
- [[60-Videojuegos/03 Unity/_Índice Unity|Unity]]
- [[60-Videojuegos/04 Game design/_Índice Game design|Game design]]
- [[60-Videojuegos/05 Arte y sonido/_Índice Arte y sonido|Arte y sonido]]
- [[60-Videojuegos/06 Programación de juegos/_Índice Programación de juegos|Programación de juegos]]
- [[60-Videojuegos/07 Publicar/_Índice Publicar|Publicar]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("60-Videojuegos")
    - tipo != "indice"
views:
  - type: table
    name: Pendientes
    filters:
      and:
        - progreso != "visto"
        - progreso != "dominado"
    order:
      - file.name
      - file.folder
      - progreso
      - nivel
  - type: table
    name: Vistos
    filters:
      and:
        - progreso == "visto"
    order:
      - file.name
      - file.folder
  - type: table
    name: Dominados
    filters:
      and:
        - progreso == "dominado"
    order:
      - file.name
      - file.folder
```
