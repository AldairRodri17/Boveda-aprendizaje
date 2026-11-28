---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Astronomía y astrofotografía
tags: [indice, astronomia]
contenido: completo
---
# Edición

Parte de: [[64-Astronomia/_Índice Astronomía y astrofotografía|Astronomía y astrofotografía]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Procesar astrofotografía]]
- [[Reducción de ruido en astrofotografía]]

## Progreso

```base
filters:
  and:
    - file.inFolder("64-Astronomia/05 Edición")
    - file.name != "_Índice Edición"
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
