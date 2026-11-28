---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Mitología e historia mesoamericana y griega
tags: [indice, mitologia]
---
# Para tus proyectos

Parte de: [[67-Mitologia/_Índice Mitología e historia mesoamericana y griega|Mitología e historia mesoamericana y griega]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[La historia detrás del nombre de mis proyectos]] ⭐ 🛠
- [[Ideas de nombres para proyectos futuros]]

## Laboratorios

- [[Lab 3 - Escribir la historia del nombre de cada proyecto]]

## Progreso

```base
filters:
  and:
    - file.inFolder("67-Mitologia/08 Para tus proyectos")
    - file.name != "_Índice Para tus proyectos"
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
