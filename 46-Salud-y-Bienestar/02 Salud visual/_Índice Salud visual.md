---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Salud y bienestar
tags: [indice, salud]
---
# Salud visual

Parte de: [[46-Salud-y-Bienestar/_Índice Salud y bienestar|Salud y bienestar]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Regla 20-20-20 y fatiga visual]] ⭐
- [[Luz de pantalla y del ambiente]]

## Progreso

```base
filters:
  and:
    - file.inFolder("46-Salud-y-Bienestar/02 Salud visual")
    - file.name != "_Índice Salud visual"
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
