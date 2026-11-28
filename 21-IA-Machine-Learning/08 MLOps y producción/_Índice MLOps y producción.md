---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: IA y Machine Learning
tags: [indice, ia-machine-learning]
contenido: completo
---
# MLOps y producción

Parte de: [[21-IA-Machine-Learning/_Índice IA y Machine Learning|IA y Machine Learning]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[MLOps - conceptos]]
- [[Versionado de datos y modelos]]
- [[MLflow]]
- [[Desplegar un modelo como API]]
- [[Monitoreo de modelos y drift]]
- [[Azure Machine Learning]]

## Laboratorios

- [[Lab 8 - Desplegar un modelo como API con FastAPI y MLflow]]

## Progreso

```base
filters:
  and:
    - file.inFolder("21-IA-Machine-Learning/08 MLOps y producción")
    - file.name != "_Índice MLOps y producción"
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
