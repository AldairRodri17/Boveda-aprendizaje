---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Aprender a aprender y productividad
tags: [indice, productividad]
---
# Gestión del tiempo

Parte de: [[43-Aprender-y-Productividad/_Índice Aprender a aprender y productividad|Aprender a aprender y productividad]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Priorizar con la matriz de Eisenhower]] ⭐
- [[Bloques de tiempo (time blocking)]] ⭐
- [[Técnica Pomodoro]]
- [[Planeación semanal]] ⭐
- [[Decir que no a compromisos]]

## Laboratorios

- [[Lab 2 - Una semana con bloques de tiempo]]

## Progreso

```base
filters:
  and:
    - file.inFolder("43-Aprender-y-Productividad/02 Gestión del tiempo")
    - file.name != "_Índice Gestión del tiempo"
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
