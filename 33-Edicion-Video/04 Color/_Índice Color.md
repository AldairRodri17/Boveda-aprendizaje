---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Edición de video
tags: [indice, edicion-video]
contenido: completo
---
# Color

Parte de: [[33-Edicion-Video/_Índice Edición de video|Edición de video]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Corrección de color vs gradación]]
- [[Scopes (waveform, vectorscopio y parade)]]
  - [[Waveform en corrección de color]]
  - [[Vectorscopio]]
  - [[Parade RGB]]
- [[Material Log y LUTs]]
- [[Nodos de color en DaVinci Resolve]]
- [[Igualar tomas y tonos de piel]]

## Laboratorios

- [[Lab 5 - Corrección y gradación de Apple Log con scopes]]

## Progreso

```base
filters:
  and:
    - file.inFolder("33-Edicion-Video/04 Color")
    - file.name != "_Índice Color"
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
