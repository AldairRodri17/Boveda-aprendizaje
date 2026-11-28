---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Negociación e influencia
tags: [indice, negociacion]
contenido: completo
---
# Negociación

Parte de: [[44-Negociacion-e-Influencia/_Índice Negociación e influencia|Negociación e influencia]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Principios de negociación (posiciones vs intereses)]] ⭐
- [[BATNA (tu mejor alternativa)]] ⭐
- [[Anclaje y primeras ofertas]]
- [[Negociación salarial]] (vive en 14 · Carrera)
- [[Negociar precios y contratos]]
- [[Negociar en el día a día (renta, compras y familia)]]

## Laboratorios

- [[Lab 1 - Preparar una negociación real con BATNA]]

## Progreso

```base
filters:
  and:
    - file.inFolder("44-Negociacion-e-Influencia/01 Negociación")
    - file.name != "_Índice Negociación"
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
