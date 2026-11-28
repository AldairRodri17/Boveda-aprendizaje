---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Azure
tags: [indice, azure]
contenido: completo
---
# Seguridad

Parte de: [[07-Azure/_Índice Azure|Azure]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Key Vault]]
- [[Managed Identity]]
  - [[Managed Identity asignada por el sistema]]
  - [[Managed Identity asignada por el usuario]]
- [[Microsoft Defender (conceptos)]]

## Laboratorios

- [[Lab 4 - Key Vault y Managed Identity]]

## Progreso

```base
filters:
  and:
    - file.inFolder("07-Azure/06 Seguridad")
    - file.name != "_Índice Seguridad"
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
