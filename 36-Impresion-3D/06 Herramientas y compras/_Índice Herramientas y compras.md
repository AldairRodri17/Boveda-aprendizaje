---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Impresión 3D
tags: [indice, impresion-3d]
---
# Herramientas y compras

Parte de: [[36-Impresion-3D/_Índice Impresión 3D|Impresión 3D]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Herramientas básicas para impresión 3D]]
- [[Calibrador digital (vernier)]]
- [[Secador de filamento]]
- [[Tablet para escultura digital (opcional)]]

## Progreso

```base
filters:
  and:
    - file.inFolder("36-Impresion-3D/06 Herramientas y compras")
    - file.name != "_Índice Herramientas y compras"
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
