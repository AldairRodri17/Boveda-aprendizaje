---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Edición de video
tags: [indice, edicion-video]
contenido: completo
---
# Fundamentos

Parte de: [[33-Edicion-Video/_Índice Edición de video|Edición de video]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[DaVinci Resolve (versión gratuita)]]
- [[Kdenlive y Shotcut]]
- [[CapCut]]
- [[Proyecto, bins y línea de tiempo]]
- [[Proxies y rendimiento al editar]]
- [[Organizar material de video]]

## Laboratorios

- [[Lab 1 - Primer proyecto organizado en DaVinci Resolve]]

## Progreso

```base
filters:
  and:
    - file.inFolder("33-Edicion-Video/01 Fundamentos")
    - file.name != "_Índice Fundamentos"
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
