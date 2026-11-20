---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Inversiones
tags: [indice, inversiones]
---
# Psicología y errores

Parte de: [[42-Inversiones/_Índice Inversiones|Inversiones]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Sesgos al invertir]]
- [[Qué hacer cuando el mercado cae]] ⭐
- [[Consejos de redes sociales y finfluencers]]
- [[Errores comunes al empezar a invertir]]

## Progreso

```base
filters:
  and:
    - file.inFolder("42-Inversiones/07 Psicología y errores")
    - file.name != "_Índice Psicología y errores"
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
