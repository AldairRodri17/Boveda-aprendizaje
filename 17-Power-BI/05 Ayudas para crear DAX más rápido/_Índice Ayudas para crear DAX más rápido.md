---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Power BI
tags: [indice, power-bi]
contenido: completo
---
# Ayudas para crear DAX más rápido

Parte de: [[17-Power-BI/_Índice Power BI|Power BI]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Medidas rápidas (quick measures)]]
- [[Vista de consultas DAX (DAX query view)]]
- [[Copilot en la vista de consultas DAX (escribir y explicar consultas)]]
- [[Sugerencias de medidas rápidas con Copilot]]
- [[Requisitos y licencias para usar Copilot en Power BI]]
- [[Cuándo confiar en Copilot y cómo validar lo que genera]]
- [[DAX Studio]]
- [[Tabular Editor]]
- [[DAX Formatter y guías de estilo]]
- [[SQLBI y otros recursos para aprender DAX]]

## Laboratorios

- [[Lab 7 - Consultas DAX con la vista de consultas y DAX Studio]]

## Progreso

```base
filters:
  and:
    - file.inFolder("17-Power-BI/05 Ayudas para crear DAX más rápido")
    - file.name != "_Índice Ayudas para crear DAX más rápido"
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
