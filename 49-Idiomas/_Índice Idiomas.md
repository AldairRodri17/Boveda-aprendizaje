---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Idiomas
tags: [indice, idiomas]
contenido: completo
---
# Idiomas

Avance y prioridades en [[Temario]].

> [!note]
> Empezamos por inglés, por niveles del Marco Común Europeo. Calificación con la [[Rúbrica de inglés]]. Para el inglés del trabajo, ver 14 · Inglés técnico.

## Secciones

- [[49-Idiomas/01 Diagnóstico y plan/_Índice Diagnóstico y plan|Diagnóstico y plan]]
- [[49-Idiomas/02 Inglés A1/_Índice Inglés A1|Inglés A1]]
- [[49-Idiomas/03 Inglés A2/_Índice Inglés A2|Inglés A2]]
- [[49-Idiomas/04 Inglés B1/_Índice Inglés B1|Inglés B1]]
- [[49-Idiomas/05 Inglés B2/_Índice Inglés B2|Inglés B2]]
- [[49-Idiomas/06 Inglés C1/_Índice Inglés C1|Inglés C1]]
- [[49-Idiomas/07 Pronunciación/_Índice Pronunciación|Pronunciación]]
- [[49-Idiomas/08 Habilidades/_Índice Habilidades|Habilidades]]
- [[49-Idiomas/09 Recursos/_Índice Recursos|Recursos]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("49-Idiomas")
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
