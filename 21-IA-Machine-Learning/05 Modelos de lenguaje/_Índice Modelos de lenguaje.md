---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: IA y Machine Learning
tags: [indice, ia-machine-learning]
contenido: completo
---
# Modelos de lenguaje

Parte de: [[21-IA-Machine-Learning/_Índice IA y Machine Learning|IA y Machine Learning]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Cómo funciona un LLM (tokens, contexto y predicción)]]
- [[Parámetros de generación (temperatura y top-p)]]
- [[Prompting efectivo]]
- [[Salidas estructuradas (JSON)]]
- [[Tool calling (llamadas a herramientas)]]
- [[RAG (generación aumentada con recuperación)]]
- [[Embeddings y bases vectoriales (pgvector y Azure AI Search)]]
- [[Chunking y reranking]]
- [[Fine-tuning vs RAG vs prompting]]
- [[LoRA y ajuste eficiente]]
- [[Alucinaciones y cómo reducirlas]]
- [[Evaluación de respuestas de LLMs]]
- [[Costo y latencia de LLMs]]
- [[Modelos abiertos vs comerciales]]

## Laboratorios

- [[Lab 4 - Embeddings y búsqueda por similitud]]
- [[Lab 5 - RAG mínimo con evaluación de recuperación]]

## Progreso

```base
filters:
  and:
    - file.inFolder("21-IA-Machine-Learning/05 Modelos de lenguaje")
    - file.name != "_Índice Modelos de lenguaje"
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
