---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Azure
tags: [indice, azure]
contenido: completo
---
# Integración

Parte de: [[07-Azure/_Índice Azure|Azure]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[API Management]]
- [[Azure DevOps REST API]]

## Progreso

```base
filters:
  and:
    - file.inFolder("07-Azure/05 Integración")
    - file.name != "_Índice Integración"
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
