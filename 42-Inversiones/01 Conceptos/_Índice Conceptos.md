---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Inversiones
tags: [indice, inversiones]
contenido: completo
---
# Conceptos

Parte de: [[42-Inversiones/_Índice Inversiones|Inversiones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Interés compuesto]] ⭐
- [[Inflación y rendimiento real]] ⭐
- [[Riesgo, rendimiento y liquidez]] ⭐
- [[Diversificación]] ⭐
- [[Perfil de inversionista]]
- [[Horizonte de inversión]]

## Laboratorios

- [[Lab 1 - Interés compuesto en una hoja de cálculo]]

## Progreso

```base
filters:
  and:
    - file.inFolder("42-Inversiones/01 Conceptos")
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
