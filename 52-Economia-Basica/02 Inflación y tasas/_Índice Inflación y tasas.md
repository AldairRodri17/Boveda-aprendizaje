---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Economía básica
tags: [indice, economia]
---
# Inflación y tasas

Parte de: [[52-Economia-Basica/_Índice Economía básica|Economía básica]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Inflación e INPC]] ⭐
- [[Tasa de interés de Banxico]] ⭐
- [[Cómo afecta la tasa a créditos e inversiones]] ⭐
- [[Banco de México y su función]]

## Laboratorios

- [[Lab 1 - Cómo afecta la tasa de Banxico a mis Cetes (con datos reales)]]

## Progreso

```base
filters:
  and:
    - file.inFolder("52-Economia-Basica/02 Inflación y tasas")
    - file.name != "_Índice Inflación y tasas"
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
