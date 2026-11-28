---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Fotografía
tags: [indice, fotografia]
contenido: completo
---
# Enfoque

Parte de: [[30-Fotografia/_Índice Fotografía|Fotografía]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Modos de enfoque automático (AF-S, AF-C y seguimiento)]]
- [[Enfoque manual y focus peaking]]
- [[Distancia hiperfocal]]
- [[Focus stacking (apilado de enfoque)]]

## Laboratorios

- [[Práctica Foto 04 - Enfoque manual, macro y apilado]]

## Progreso

```base
filters:
  and:
    - file.inFolder("30-Fotografia/03 Enfoque")
    - file.name != "_Índice Enfoque"
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
