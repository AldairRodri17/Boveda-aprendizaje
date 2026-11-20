---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Radar de herramientas
tags: [indice, radar-herramientas]
---
# Radar de herramientas

Avance general en [[Temario]] · Mapa mental: [[Mapa mental - Radar de herramientas.canvas|Mapa mental]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Radar - cómo usarlo]]
- [[Radar - IA para desarrollo]]
- [[Radar - observabilidad]]
- [[Radar - infraestructura como código]]
- [[Radar - bases de datos]]
- [[Radar - testing]]
- [[Radar - experiencia de desarrollo]]
- [[Postman, Bruno y archivos .http]]
- [[SSMS, Azure Data Studio y DBeaver]]
- [[.NET Aspire]]
- [[OpenTelemetry]]
- [[Mermaid y diagramas como código]]
- [[Obsidian (frontmatter, Dataview y plugins útiles)]]

## Laboratorios

- [[Lab 1 - Armar mi radar personal de herramientas]]
- [[Lab 2 - Probar un API con archivos .http y Bruno]]

## Progreso

```base
filters:
  and:
    - file.inFolder("10-Radar-Herramientas")
    - file.name != "_Índice Radar de herramientas"
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

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("10-Radar-Herramientas")
    - tipo != "indice"
views:
  - type: table
    name: Pendientes
    filters:
      and:
        - progreso != "visto"
        - progreso != "dominado"
    order:
      - file.name
      - file.folder
      - progreso
      - nivel
  - type: table
    name: Vistos
    filters:
      and:
        - progreso == "visto"
    order:
      - file.name
      - file.folder
  - type: table
    name: Dominados
    filters:
      and:
        - progreso == "dominado"
    order:
      - file.name
      - file.folder
```
