---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Power BI
tags: [indice, power-bi]
contenido: completo
---
# DAX

Parte de: [[17-Power-BI/_Índice Power BI|Power BI]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[DAX - qué es y para qué sirve]]
- [[DAX - columnas calculadas, medidas y tablas calculadas]]
- [[DAX - sintaxis y operadores]]
- [[DAX - contexto de fila]]
- [[DAX - contexto de filtro]]
- [[DAX - transición de contexto]]
- [[DAX - CALCULATE]]
- [[DAX - FILTER, ALL, ALLEXCEPT y REMOVEFILTERS]]
- [[DAX - VALUES y DISTINCT]]
- [[DAX - iteradores (SUMX, AVERAGEX y demás)]]
- [[DAX - RELATED y RELATEDTABLE]]
- [[DAX - variables (VAR y RETURN)]]
- [[DAX - IF, SWITCH y DIVIDE]]
- [[DAX - conteos (COUNTROWS y DISTINCTCOUNT)]]
- [[DAX - inteligencia de tiempo]]
- [[DAX - YTD, mismo periodo del año anterior y comparativos]]
- [[DAX - RANKX y TOPN]]
- [[DAX - USERELATIONSHIP, TREATAS y CROSSFILTER]]
- [[DAX - SELECTEDVALUE, ISINSCOPE y HASONEVALUE]]
- [[DAX - tablas virtuales]]
- [[DAX - SUMMARIZE, SUMMARIZECOLUMNS y ADDCOLUMNS]]
- [[DAX - totales acumulados]]
- [[DAX - medidas semiaditivas]]
- [[DAX - patrones comunes (ABC, Pareto y cohortes)]]
- [[DAX - seguridad dinámica con USERPRINCIPALNAME]]
- [[DAX - errores comunes]]
- [[DAX - optimización]]
- [[Consultas DAX (EVALUATE, DEFINE y MEASURE)]]
- [[Grupos de cálculo]]
- [[Parámetros de campo]]
- [[Cálculos visuales (visual calculations)]]

## Laboratorios

- [[Lab 6 - Medidas con CALCULATE y comparativos de tiempo en Power BI]]

## Progreso

```base
filters:
  and:
    - file.inFolder("17-Power-BI/04 DAX")
    - file.name != "_Índice DAX"
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
