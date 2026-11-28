---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Idiomas
tags: [indice, idiomas]
---
# Recursos

Parte de: [[49-Idiomas/_Índice Idiomas|Idiomas]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Series, podcasts y YouTube para aprender inglés por nivel]]
- [[Apps para aprender inglés]]
- [[Diario en inglés]]
- [[Intercambio de idiomas]]
- [[Certificaciones de inglés (TOEFL, IELTS, Cambridge y EF SET)]] (vive en 18)

## Progreso

```base
filters:
  and:
    - file.inFolder("49-Idiomas/09 Recursos")
    - file.name != "_Índice Recursos"
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
