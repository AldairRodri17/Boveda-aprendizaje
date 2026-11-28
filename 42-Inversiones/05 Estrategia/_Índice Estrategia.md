---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Inversiones
tags: [indice, inversiones]
---
# Estrategia

Parte de: [[42-Inversiones/_Índice Inversiones|Inversiones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Asignación de activos por edad y metas]] ⭐
- [[Inversión periódica (DCA)]] ⭐
- [[Rebalanceo de portafolio]]
- [[Largo plazo vs especulación]]
- [[Invertir en dólares y riesgo cambiario]]

## Laboratorios

- [[Lab 3 - Diseñar mi asignación de activos en papel]]

## Progreso

```base
filters:
  and:
    - file.inFolder("42-Inversiones/05 Estrategia")
    - file.name != "_Índice Estrategia"
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
