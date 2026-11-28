---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Azure
tags: [indice, azure]
contenido: completo
---
# Azure DevOps

Parte de: [[07-Azure/_Índice Azure|Azure]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Azure DevOps - organización y proyectos]]
- [[Azure Repos]]
- [[Azure Pipelines]]
- [[Azure Pipelines - agentes]]
- [[Azure Pipelines - service connections]]
- [[Azure Pipelines - variable groups]]
- [[Azure Pipelines - environments y aprobaciones]]
- [[Azure Pipelines - plantillas]]
- [[Azure Boards]]
- [[Azure Artifacts]]
- [[Azure Test Plans]]

## Laboratorios

- [[Lab 5 - Pipeline YAML con stages y aprobaciones]]

## Progreso

```base
filters:
  and:
    - file.inFolder("07-Azure/09 Azure DevOps")
    - file.name != "_Índice Azure DevOps"
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
