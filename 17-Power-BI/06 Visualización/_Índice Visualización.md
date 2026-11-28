---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Power BI
tags: [indice, power-bi]
contenido: completo
---
# Visualización

Parte de: [[17-Power-BI/_Índice Power BI|Power BI]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Tipos de gráficos y cuándo usar cada uno]]
- [[Principios de diseño de informes]]
- [[Colores y accesibilidad en informes]]
- [[Segmentaciones de datos (slicers)]]
- [[Filtros de visual, página e informe]]
- [[Interacciones entre visuales]]
- [[Información sobre herramientas (tooltips)]]
- [[Drill down y drill through]]
- [[Marcadores y botones]]
- [[Formato condicional]]
- [[Temas y archivos JSON de tema]]
- [[Diseño para móvil]]
- [[Informes paginados (Report Builder)]]
- [[Narrativa con datos (storytelling)]]

## Laboratorios

- [[Lab 8 - Informe interactivo con marcadores, drill through y tooltips]]

## Progreso

```base
filters:
  and:
    - file.inFolder("17-Power-BI/06 Visualización")
    - file.name != "_Índice Visualización"
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
