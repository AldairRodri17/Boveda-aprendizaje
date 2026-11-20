---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Fotografía
tags: [indice, fotografia]
---
# Avanzado

Parte de: [[30-Fotografia/_Índice Fotografía|Fotografía]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Filtros (ND, polarizador y degradado)]]
  - [[Filtro ND en fotografía]]
  - [[Filtro polarizador circular (CPL)]]
  - [[Filtro degradado (GND)]]
- [[Fotografía panorámica]]
- [[Time-lapse]]
- [[Fotografía de estudio y tethering]]
- [[Fotografía computacional en celulares]]

## Laboratorios

- [[Práctica Foto 10 - Panorámica y time-lapse]]

## Progreso

```base
filters:
  and:
    - file.inFolder("30-Fotografia/07 Avanzado")
    - file.name != "_Índice Avanzado"
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
