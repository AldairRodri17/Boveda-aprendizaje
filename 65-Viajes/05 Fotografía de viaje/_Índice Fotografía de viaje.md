---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Viajes y fotografía de viaje
tags: [indice, viajes]
---
# Fotografía de viaje

Parte de: [[65-Viajes/_Índice Viajes y fotografía de viaje|Viajes y fotografía de viaje]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Fotografía de viaje]] (vive en 30 · Fotografía)
- [[Contar la historia de un viaje (fotos y video)]]
- [[Fotografiar personas con respeto]]
- [[Planear tomas (hora dorada, clima y lugares)]]

## Laboratorios

- [[Lab 2 - Mini documental de un viaje]]

## Progreso

```base
filters:
  and:
    - file.inFolder("65-Viajes/05 Fotografía de viaje")
    - file.name != "_Índice Fotografía de viaje"
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
