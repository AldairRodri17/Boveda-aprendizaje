---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Salud y bienestar
tags: [indice, salud]
contenido: completo
---
# Alimentación

Parte de: [[46-Salud-y-Bienestar/_Índice Salud y bienestar|Salud y bienestar]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Alimentación y energía durante el día]]
- [[Hidratación]]
- [[Cafeína con criterio]]

## Progreso

```base
filters:
  and:
    - file.inFolder("46-Salud-y-Bienestar/05 Alimentación")
    - file.name != "_Índice Alimentación"
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
