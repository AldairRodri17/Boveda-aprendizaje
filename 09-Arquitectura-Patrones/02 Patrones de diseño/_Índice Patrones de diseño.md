---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Arquitectura y patrones
tags: [indice, arquitectura-patrones]
contenido: completo
---
# Patrones de diseño

Parte de: [[09-Arquitectura-Patrones/_Índice Arquitectura y patrones|Arquitectura y patrones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Patrón Strategy]]
- [[Patrón Factory]]
- [[Patrón Builder]]
- [[Patrón Mediator]]
- [[Patrón Template Method]]
- [[Patrón Specification]]
- [[Patrón Observer]]
- [[Patrón Decorator]]
- [[Patrón Adapter]]
- [[Patrón Singleton]]
- [[Result pattern vs excepciones]]
- [[Guard clauses]]

## Laboratorios

- [[Lab 1 - Refactor de un switch a Strategy]]

## Progreso

```base
filters:
  and:
    - file.inFolder("09-Arquitectura-Patrones/02 Patrones de diseño")
    - file.name != "_Índice Patrones de diseño"
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
