---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Filosofía práctica
tags: [indice, filosofia]
---
# Práctica

Parte de: [[66-Filosofia-Practica/_Índice Filosofía práctica|Filosofía práctica]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Diario filosófico]] ⭐
- [[Lecturas recomendadas de filosofía práctica]]
- [[Aplicar la filosofía en el trabajo]]

## Progreso

```base
filters:
  and:
    - file.inFolder("66-Filosofia-Practica/06 Práctica")
    - file.name != "_Índice Práctica"
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
