---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Idiomas
tags: [indice, idiomas]
---
# Diagnóstico y plan

Parte de: [[49-Idiomas/_Índice Idiomas|Idiomas]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Niveles del Marco Común Europeo (A1 a C2)]] ⭐
- [[Examen de colocación de inglés]] ⭐
- [[Rutina diaria de inglés (20 a 30 minutos)]] ⭐
- [[Rúbrica de inglés]] ⭐ _(existe)_
- [[Inglés técnico - plan de estudio]] (vive en 14 · Carrera)

## Laboratorios

- [[Lab 1 - Examen de colocación con Claude y EF SET]]

## Progreso

```base
filters:
  and:
    - file.inFolder("49-Idiomas/01 Diagnóstico y plan")
    - file.name != "_Índice Diagnóstico y plan"
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
