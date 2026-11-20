---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Power BI
tags: [indice, power-bi]
---
# Avanzado

Parte de: [[17-Power-BI/_Índice Power BI|Power BI]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Power BI Embedded (incrustar en una aplicación React o .NET)]]
- [[API REST de Power BI]]
- [[Cliente JavaScript de Power BI]]
- [[Punto de conexión XMLA]]
- [[TMDL y proyectos PBIP]]
- [[Power BI con Git y control de versiones]]
- [[Canalizaciones de implementación (deployment pipelines)]]
- [[ALM Toolkit]]
- [[Agregaciones]]
- [[Rendimiento - Performance Analyzer y VertiPaq Analyzer]]
- [[Modelos grandes]]
- [[Fabric - Lakehouse, Warehouse y OneLake]]
- [[Direct Lake]]
- [[Power BI con Azure (SQL, Synapse y Analysis Services)]]
- [[Power BI con Power Automate y Power Apps]]
- [[Gobierno de datos en Power BI]]

## Progreso

```base
filters:
  and:
    - file.inFolder("17-Power-BI/09 Avanzado")
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
