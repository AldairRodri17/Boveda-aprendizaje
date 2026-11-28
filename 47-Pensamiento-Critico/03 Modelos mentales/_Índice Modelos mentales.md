---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Pensamiento crítico y toma de decisiones
tags: [indice, pensamiento-critico]
---
# Modelos mentales

Parte de: [[47-Pensamiento-Critico/_Índice Pensamiento crítico y toma de decisiones|Pensamiento crítico y toma de decisiones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Pensar desde primeros principios]] ⭐
- [[Pensamiento de segundo orden]] ⭐
- [[Inversión (pensar al revés)]]
- [[Costo de oportunidad]] ⭐
- [[Navaja de Occam y navaja de Hanlon]]
- [[Círculo de competencia]]

## Progreso

```base
filters:
  and:
    - file.inFolder("47-Pensamiento-Critico/03 Modelos mentales")
    - file.name != "_Índice Modelos mentales"
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
