---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: IA y Machine Learning
tags: [indice, ia-machine-learning]
---
# IA y Machine Learning

Avance y prioridades en [[Temario]]. Mapa visual: [[Mapa mental - IA y Machine Learning.canvas|Mapa mental]].

## Secciones

- [[21-IA-Machine-Learning/01 Matemáticas necesarias/_Índice Matemáticas necesarias|Matemáticas necesarias]]
- [[21-IA-Machine-Learning/02 Python para datos/_Índice Python para datos|Python para datos]]
- [[21-IA-Machine-Learning/03 Machine learning clásico/_Índice Machine learning clásico|Machine learning clásico]]
- [[21-IA-Machine-Learning/04 Redes neuronales y deep learning/_Índice Redes neuronales y deep learning|Redes neuronales y deep learning]]
- [[21-IA-Machine-Learning/05 Modelos de lenguaje/_Índice Modelos de lenguaje|Modelos de lenguaje]]
- [[21-IA-Machine-Learning/06 Agentes de IA/_Índice Agentes de IA|Agentes de IA]]
- [[21-IA-Machine-Learning/07 IA en .NET y Azure/_Índice IA en .NET y Azure|IA en .NET y Azure]]
- [[21-IA-Machine-Learning/08 MLOps y producción/_Índice MLOps y producción|MLOps y producción]]
- [[21-IA-Machine-Learning/09 Responsabilidad y seguridad/_Índice Responsabilidad y seguridad|Responsabilidad y seguridad]]
- [[21-IA-Machine-Learning/10 Recursos y práctica/_Índice Recursos y práctica|Recursos y práctica]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("21-IA-Machine-Learning")
    - tipo != "indice"
views:
  - type: table
    name: Pendientes
    filters:
      and:
        - progreso != "visto"
        - progreso != "dominado"
    order:
      - file.name
      - file.folder
      - progreso
      - nivel
  - type: table
    name: Vistos
    filters:
      and:
        - progreso == "visto"
    order:
      - file.name
      - file.folder
  - type: table
    name: Dominados
    filters:
      and:
        - progreso == "dominado"
    order:
      - file.name
      - file.folder
```
