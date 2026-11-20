---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Hablar en público y exposiciones
tags: [indice, hablar-en-publico]
---
# Formatos

Parte de: [[40-Hablar-en-Publico/_Índice Hablar en público y exposiciones|Hablar en público y exposiciones]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Exposición técnica a desarrolladores]] ⭐
- [[Presentar a directivos (resumen ejecutivo)]] (vive en 14 · Carrera)
- [[Pitch de 2 minutos]] ⭐
- [[Charla en meetup o conferencia]]
- [[Lightning talk]]
- [[Hablar en juntas y reuniones]]
- [[Improvisar (hablar sin preparación)]] ⭐

## Laboratorios

- [[Lab 2 - Exposición técnica de 5 minutos con ronda de preguntas]]
- [[Lab 3 - Improvisación de 1 minuto con tema al azar]]

## Progreso

```base
filters:
  and:
    - file.inFolder("40-Hablar-en-Publico/05 Formatos")
    - file.name != "_Índice Formatos"
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
