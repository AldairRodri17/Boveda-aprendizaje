---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Economía básica
tags: [indice, economia]
---
# Tipo de cambio y comercio

Parte de: [[52-Economia-Basica/_Índice Economía básica|Economía básica]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Tipo de cambio peso-dólar]] ⭐
- [[Remesas, exportaciones e importaciones]]
- [[T-MEC y comercio internacional]]

## Progreso

```base
filters:
  and:
    - file.inFolder("52-Economia-Basica/03 Tipo de cambio y comercio")
    - file.name != "_Índice Tipo de cambio y comercio"
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
