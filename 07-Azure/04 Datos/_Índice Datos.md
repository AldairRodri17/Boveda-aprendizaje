---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Azure
tags: [indice, azure]
---
# Datos

Parte de: [[07-Azure/_Índice Azure|Azure]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Azure SQL Database]]
- [[Cosmos DB]]
- [[Azure Blob Storage]]
  - [[Blob Storage - SAS (firmas de acceso compartido)]]
  - [[Blob Storage - niveles de acceso y ciclo de vida]]
- [[Queue Storage y Table Storage]]
- [[Azure Cache for Redis]]
- [[Service Bus]]
- [[Event Grid]]
- [[Event Hubs]]
- [[Azure AI Search]]

## Laboratorios

- [[Lab 3 - Blob Storage con SAS]]

## Progreso

```base
filters:
  and:
    - file.inFolder("07-Azure/04 Datos")
    - file.name != "_Índice Datos"
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
