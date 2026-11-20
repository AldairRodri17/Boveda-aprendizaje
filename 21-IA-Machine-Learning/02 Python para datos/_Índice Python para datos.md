---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: IA y Machine Learning
tags: [indice, ia-machine-learning]
---
# Python para datos

Parte de: [[21-IA-Machine-Learning/_Índice IA y Machine Learning|IA y Machine Learning]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Python - básico]]
- [[Python - entornos virtuales y pip]]
- [[Jupyter notebooks]]
- [[NumPy]]
- [[pandas]]
- [[Visualización con matplotlib y seaborn]]

## Laboratorios

- [[Lab 1 - NumPy y pandas en Google Colab]]

## Progreso

```base
filters:
  and:
    - file.inFolder("21-IA-Machine-Learning/02 Python para datos")
    - file.name != "_Índice Python para datos"
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
