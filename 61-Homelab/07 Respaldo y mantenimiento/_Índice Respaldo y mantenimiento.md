---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Homelab y self-hosting
tags: [indice, homelab]
---
# Respaldo y mantenimiento

Parte de: [[61-Homelab/_Índice Homelab y self-hosting|Homelab y self-hosting]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Respaldo 3-2-1 del homelab]] ⭐
- [[Actualizaciones y mantenimiento del homelab]]
- [[Documentar tu homelab]]
- [[UPS y cortes de luz]]

## Progreso

```base
filters:
  and:
    - file.inFolder("61-Homelab/07 Respaldo y mantenimiento")
    - file.name != "_Índice Respaldo y mantenimiento"
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
