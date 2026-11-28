---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Power BI
tags: [indice, power-bi]
contenido: completo
---
# Fundamentos

Parte de: [[17-Power-BI/_Índice Power BI|Power BI]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Qué es la inteligencia de negocios (BI)]]
- [[Ecosistema de Power BI (Desktop, Service, Mobile, Report Server y Embedded)]]
- [[Power BI y Microsoft Fabric]]
- [[Licencias de Power BI (Free, Pro, Premium por usuario y capacidades)]]
- [[Instalar Power BI Desktop y primer informe]]
- [[Flujo de trabajo (obtener, transformar, modelar, visualizar y publicar)]]
- [[Mapa de lenguajes de Power BI (DAX, M, SQL, R, Python, Vega-Lite, TypeScript y JSON)]]
- [[Conectores y orígenes de datos (Excel, CSV, SQL Server, Web y API)]]
- [[Import, DirectQuery, Dual, Live Connection y Direct Lake]]
  - [[Modo de almacenamiento Import]]
  - [[Modo de almacenamiento DirectQuery]]
  - [[Modo de almacenamiento Dual]]
  - [[Conexión dinámica (Live Connection) en Power BI]]
- [[Modelos compuestos]]

## Laboratorios

- [[Lab 1 - Primer informe de Power BI desde Excel o CSV]]

## Progreso

```base
filters:
  and:
    - file.inFolder("17-Power-BI/01 Fundamentos")
    - file.name != "_Índice Fundamentos"
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
