---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: IA y Machine Learning
tags: [indice, ia-machine-learning]
---
# Responsabilidad y seguridad

Parte de: [[21-IA-Machine-Learning/_Índice IA y Machine Learning|IA y Machine Learning]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[IA responsable (sesgo, equidad y transparencia)]]
- [[Prompt injection]]
- [[OWASP Top 10 para LLMs]]
- [[Privacidad de datos al usar IA]]

## Progreso

```base
filters:
  and:
    - file.inFolder("21-IA-Machine-Learning/09 Responsabilidad y seguridad")
    - file.name != "_Índice Responsabilidad y seguridad"
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
