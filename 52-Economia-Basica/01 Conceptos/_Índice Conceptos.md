---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Economía básica
tags: [indice, economia]
---
# Conceptos

Parte de: [[52-Economia-Basica/_Índice Economía básica|Economía básica]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Qué es la economía (micro y macro)]]
- [[Oferta y demanda]] ⭐
- [[El dinero y para qué sirve]]
- [[PIB y crecimiento económico]]

## Progreso

```base
filters:
  and:
    - file.inFolder("52-Economia-Basica/01 Conceptos")
    - file.name != "_Índice Conceptos"
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
