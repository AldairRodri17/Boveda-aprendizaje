---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: IA y Machine Learning
tags: [indice, ia-machine-learning]
---
# IA en .NET y Azure

Parte de: [[21-IA-Machine-Learning/_Índice IA y Machine Learning|IA y Machine Learning]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Microsoft.Extensions.AI]]
- [[Semantic Kernel y Microsoft Agent Framework]]
- [[ML.NET]]
- [[ONNX Runtime]]
- [[Azure OpenAI y Azure AI Foundry]]

## Progreso

```base
filters:
  and:
    - file.inFolder("21-IA-Machine-Learning/07 IA en .NET y Azure")
    - file.name != "_Índice IA en .NET y Azure"
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
