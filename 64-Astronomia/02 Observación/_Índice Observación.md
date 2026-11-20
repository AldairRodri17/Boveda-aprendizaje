---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Astronomía y astrofotografía
tags: [indice, astronomia]
---
# Observación

Parte de: [[64-Astronomia/_Índice Astronomía y astrofotografía|Astronomía y astrofotografía]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Observar el cielo a simple vista]]
- [[Binoculares para astronomía]]
- [[Telescopios (tipos y monturas)]]
- [[Calendario astronómico (lluvias de meteoros y eclipses)]] ⭐
- [[Observar la Luna y los planetas]]

## Progreso

```base
filters:
  and:
    - file.inFolder("64-Astronomia/02 Observación")
    - file.name != "_Índice Observación"
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
