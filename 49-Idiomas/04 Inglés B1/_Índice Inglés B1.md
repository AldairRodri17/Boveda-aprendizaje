---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Idiomas
tags: [indice, idiomas]
---
# Inglés B1

Parte de: [[49-Idiomas/_Índice Idiomas|Idiomas]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Inglés B1 - presente perfecto]] ⭐
- [[Inglés B1 - pasado continuo]]
- [[Inglés B1 - condicionales cero, primero y segundo]] ⭐
- [[Inglés B1 - voz pasiva]]
- [[Inglés B1 - modales (must, have to y might)]]
- [[Inglés B1 - phrasal verbs más usados]] ⭐
- [[Inglés B1 - contar experiencias y dar opiniones]]

## Laboratorios

- [[Lab 2 - Conversación de 10 minutos sobre mi trabajo]]

## Progreso

```base
filters:
  and:
    - file.inFolder("49-Idiomas/04 Inglés B1")
    - file.name != "_Índice Inglés B1"
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
