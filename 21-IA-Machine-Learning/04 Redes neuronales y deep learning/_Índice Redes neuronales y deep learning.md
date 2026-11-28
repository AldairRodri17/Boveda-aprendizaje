---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: IA y Machine Learning
tags: [indice, ia-machine-learning]
contenido: completo
---
# Redes neuronales y deep learning

Parte de: [[21-IA-Machine-Learning/_Índice IA y Machine Learning|IA y Machine Learning]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Neurona artificial y perceptrón]]
- [[Funciones de activación]]
  - [[ReLU y sus variantes]]
  - [[Sigmoide y tanh]]
  - [[Softmax]]
- [[Redes neuronales multicapa]]
- [[Función de pérdida]]
- [[Descenso de gradiente y backpropagation]]
- [[Optimizadores (SGD y Adam)]]
- [[Regularización y dropout]]
- [[Redes convolucionales (CNN)]]
- [[Redes recurrentes (RNN y LSTM)]]
- [[Embeddings]]
- [[Mecanismo de atención]]
- [[Transformers]]
- [[Transfer learning y fine-tuning]]
- [[PyTorch]]
- [[GPUs y por qué importan]]

## Laboratorios

- [[Lab 3 - Red neuronal pequeña en PyTorch]]

## Progreso

```base
filters:
  and:
    - file.inFolder("21-IA-Machine-Learning/04 Redes neuronales y deep learning")
    - file.name != "_Índice Redes neuronales y deep learning"
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
