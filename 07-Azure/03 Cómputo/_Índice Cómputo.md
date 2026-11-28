---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Azure
tags: [indice, azure]
contenido: completo
---
# Cómputo

Parte de: [[07-Azure/_Índice Azure|Azure]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[App Service]]
- [[App Service - planes y escalado]]
- [[App Service - deployment slots]]
- [[Azure Functions]]
  - [[Azure Functions - triggers y bindings]]
  - [[Azure Functions - planes de hospedaje]]
- [[Azure Static Web Apps]]
- [[Container Apps]]
- [[Container Registry]]
- [[AKS (conceptos)]]
- [[Máquinas virtuales]]

## Laboratorios

- [[Lab 2 - Desplegar una API .NET a App Service]]

## Progreso

```base
filters:
  and:
    - file.inFolder("07-Azure/03 Cómputo")
    - file.name != "_Índice Cómputo"
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
