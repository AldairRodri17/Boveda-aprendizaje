---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Restauración del Vocho
tags: [indice, vocho]
---
# Interiores

Parte de: [[68-Restauracion-Vocho/_Índice Restauración del Vocho|Restauración del Vocho]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Vestiduras y asientos]]
- [[Tablero e instrumentos]]
- [[Aislamiento térmico y acústico]]
- [[Piezas de interior impresas en 3D]] (enlaza con 37)

## Laboratorios

- [[Lab 5 - Imprimir en 3D una pieza para el vocho]]

## Progreso

```base
filters:
  and:
    - file.inFolder("68-Restauracion-Vocho/08 Interiores")
    - file.name != "_Índice Interiores"
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
