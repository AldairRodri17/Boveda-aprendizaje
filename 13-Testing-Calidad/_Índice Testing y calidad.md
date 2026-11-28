---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Testing y calidad
tags: [indice, testing-calidad]
contenido: completo
---
# Testing y calidad

Avance general en [[Temario]] · Mapa mental: [[Mapa mental - Testing y calidad.canvas|Mapa mental]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Pirámide de pruebas]]
- [[Pruebas unitarias]]
- [[Pruebas de integración]]
- [[Pruebas end-to-end (Playwright y Cypress)]]
- [[Snapshot testing]]
- [[TDD]]
- [[BDD]]
- [[Cobertura de pruebas]]
- [[Qué probar y qué no]]
- [[Diseñar código fácil de probar]]
- [[Revisión de código]]
- [[Logging estructurado]]
- [[Métricas, trazas y logs]]
- [[Sentry]]
- [[Perfilado de rendimiento]]
- [[Documentación técnica]]

## Laboratorios

- [[Lab 1 - Pirámide de pruebas en un proyecto pequeño]]
- [[Lab 2 - Logging estructurado con trazas en ASP.NET Core]]

## Progreso

```base
filters:
  and:
    - file.inFolder("13-Testing-Calidad")
    - file.name != "_Índice Testing y calidad"
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
    - file.inFolder("13-Testing-Calidad")
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
