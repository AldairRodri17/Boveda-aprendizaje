---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Mitología e historia mesoamericana y griega
tags: [indice, mitologia]
---
# Comparaciones

Parte de: [[67-Mitologia/_Índice Mitología e historia mesoamericana y griega|Mitología e historia mesoamericana y griega]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Mitología mesoamericana vs griega]] ⭐
- [[Mitos de creación comparados]]

## Progreso

```base
filters:
  and:
    - file.inFolder("67-Mitologia/06 Comparaciones")
    - file.name != "_Índice Comparaciones"
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
