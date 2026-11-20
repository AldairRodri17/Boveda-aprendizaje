---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Azure
tags: [indice, azure]
---
# Infraestructura como código

Parte de: [[07-Azure/_Índice Azure|Azure]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[ARM templates]]
- [[Bicep]]
- [[Terraform]]
- [[PowerShell para Azure]]

## Laboratorios

- [[Lab 6 - Bicep básico]]

## Progreso

```base
filters:
  and:
    - file.inFolder("07-Azure/10 Infraestructura como código")
    - file.name != "_Índice Infraestructura como código"
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
