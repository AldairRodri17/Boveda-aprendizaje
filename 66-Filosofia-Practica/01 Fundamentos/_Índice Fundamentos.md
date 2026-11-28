---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Filosofía práctica
tags: [indice, filosofia]
contenido: completo
---
# Fundamentos

Parte de: [[66-Filosofia-Practica/_Índice Filosofía práctica|Filosofía práctica]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Qué es la filosofía práctica]] ⭐
- [[Las grandes preguntas de la filosofía]]
- [[Pensar con filosofía]] (enlaza con 47)

## Progreso

```base
filters:
  and:
    - file.inFolder("66-Filosofia-Practica/01 Fundamentos")
    - file.name != "_Índice Fundamentos"
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
