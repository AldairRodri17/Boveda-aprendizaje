---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Filosofía práctica
tags: [indice, filosofia]
---
# Vivir con propósito

Parte de: [[66-Filosofia-Practica/_Índice Filosofía práctica|Filosofía práctica]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Valores personales]] ⭐
- [[Propósito y sentido]]
- [[Gratitud]]
- [[Minimalismo]]

## Laboratorios

- [[Lab 2 - Definir mis valores personales]]

## Progreso

```base
filters:
  and:
    - file.inFolder("66-Filosofia-Practica/05 Vivir con propósito")
    - file.name != "_Índice Vivir con propósito"
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
