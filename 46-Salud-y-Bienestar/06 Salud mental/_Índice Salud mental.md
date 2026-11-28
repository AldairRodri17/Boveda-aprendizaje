---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Salud y bienestar
tags: [indice, salud]
contenido: completo
---
# Salud mental

Parte de: [[46-Salud-y-Bienestar/_Índice Salud y bienestar|Salud y bienestar]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Manejo del estrés]] ⭐
- [[Ansiedad (información general)]]
- [[Cuándo buscar ayuda profesional]] ⭐

## Progreso

```base
filters:
  and:
    - file.inFolder("46-Salud-y-Bienestar/06 Salud mental")
    - file.name != "_Índice Salud mental"
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
