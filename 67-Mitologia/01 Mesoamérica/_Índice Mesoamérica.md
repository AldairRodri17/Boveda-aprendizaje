---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Mitología e historia mesoamericana y griega
tags: [indice, mitologia]
---
# Mesoamérica

Parte de: [[67-Mitologia/_Índice Mitología e historia mesoamericana y griega|Mitología e historia mesoamericana y griega]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Mesoamérica - qué es y sus culturas]] ⭐
- [[Línea de tiempo de Mesoamérica]] ⭐
- [[Calendarios mesoamericanos (tonalpohualli, xiuhpohualli y cuenta larga)]]
- [[Cosmovisión mesoamericana (rumbos y niveles del universo)]]

## Laboratorios

- [[Lab 1 - Línea de tiempo de Mesoamérica]]

## Progreso

```base
filters:
  and:
    - file.inFolder("67-Mitologia/01 Mesoamérica")
    - file.name != "_Índice Mesoamérica"
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
