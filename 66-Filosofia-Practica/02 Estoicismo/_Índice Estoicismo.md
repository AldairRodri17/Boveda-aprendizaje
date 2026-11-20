---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Filosofía práctica
tags: [indice, filosofia]
---
# Estoicismo

Parte de: [[66-Filosofia-Practica/_Índice Filosofía práctica|Filosofía práctica]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Estoicismo - qué es]] ⭐
- [[Dicotomía del control]] ⭐
- [[Marco Aurelio, Séneca y Epicteto]]
- [[Ejercicios estoicos (visualización negativa, visión desde arriba y diario)]] ⭐
- [[Estoicismo y manejo del estrés]]

## Laboratorios

- [[Lab 1 - Dos semanas de diario estoico]]

## Progreso

```base
filters:
  and:
    - file.inFolder("66-Filosofia-Practica/02 Estoicismo")
    - file.name != "_Índice Estoicismo"
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
