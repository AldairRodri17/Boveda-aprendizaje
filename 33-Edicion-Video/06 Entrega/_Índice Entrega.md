---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Edición de video
tags: [indice, edicion-video]
contenido: completo
---
# Entrega

Parte de: [[33-Edicion-Video/_Índice Edición de video|Edición de video]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Exportar video (códec, bitrate y formato por plataforma)]]
- [[Miniaturas y diseño básico]]
- [[HandBrake]]
- [[OBS Studio]]
- [[Content ID y derechos en plataformas]]

## Laboratorios

- [[Lab 8 - Exportar por plataforma]]

## Progreso

```base
filters:
  and:
    - file.inFolder("33-Edicion-Video/06 Entrega")
    - file.name != "_Índice Entrega"
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
