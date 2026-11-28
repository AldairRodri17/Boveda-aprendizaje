---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Edición de video
tags: [indice, edicion-video]
contenido: completo
---
# Audio en posproducción

Parte de: [[33-Edicion-Video/_Índice Edición de video|Edición de video]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Limpieza de ruido en audio]]
- [[Ecualización y compresión de voz]]
- [[Loudness (LUFS) por plataforma]]
- [[Mezcla de voz y música]]
- [[Audacity]]
- [[Música y efectos libres de regalías]]

## Laboratorios

- [[Lab 4 - Limpieza de voz y loudness para YouTube]]

## Progreso

```base
filters:
  and:
    - file.inFolder("33-Edicion-Video/03 Audio en posproducción")
    - file.name != "_Índice Audio en posproducción"
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
