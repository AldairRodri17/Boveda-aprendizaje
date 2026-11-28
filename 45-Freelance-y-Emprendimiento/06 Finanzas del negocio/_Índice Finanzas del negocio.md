---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Freelance y emprendimiento
tags: [indice, freelance]
---
# Finanzas del negocio

Parte de: [[45-Freelance-y-Emprendimiento/_Índice Freelance y emprendimiento|Freelance y emprendimiento]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Separar finanzas personales y del negocio]] ⭐
- [[Flujo de efectivo]]
- [[Apartar para impuestos y meses flojos]]

## Progreso

```base
filters:
  and:
    - file.inFolder("45-Freelance-y-Emprendimiento/06 Finanzas del negocio")
    - file.name != "_Índice Finanzas del negocio"
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
