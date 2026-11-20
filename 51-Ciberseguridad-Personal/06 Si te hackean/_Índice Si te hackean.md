---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Ciberseguridad personal y vida digital
tags: [indice, ciberseguridad-personal]
---
# Si te hackean

Parte de: [[51-Ciberseguridad-Personal/_Índice Ciberseguridad personal y vida digital|Ciberseguridad personal y vida digital]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Qué hacer si te roban una cuenta]] ⭐
- [[Qué hacer si pierdes o te roban el celular]] ⭐
- [[Robo de identidad]]

## Progreso

```base
filters:
  and:
    - file.inFolder("51-Ciberseguridad-Personal/06 Si te hackean")
    - file.name != "_Índice Si te hackean"
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
