---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Pensamiento crítico y toma de decisiones
tags: [indice, pensamiento-critico]
---
# Decisiones

Parte de: [[47-Pensamiento-Critico/_Índice Pensamiento crítico y toma de decisiones|Pensamiento crítico y toma de decisiones]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Decidir con información incompleta]] ⭐
- [[Matriz de decisión ponderada]]
- [[Premortem]] ⭐
- [[Diario de decisiones]]
- [[Probabilidades y valor esperado]]

## Laboratorios

- [[Lab 1 - Premortem de un proyecto propio]]
- [[Lab 2 - Diario de decisiones de un mes]]

## Progreso

```base
filters:
  and:
    - file.inFolder("47-Pensamiento-Critico/04 Decisiones")
    - file.name != "_Índice Decisiones"
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
