---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Impresión 3D
tags: [indice, impresion-3d]
---
# Fundamentos

Parte de: [[36-Impresion-3D/_Índice Impresión 3D|Impresión 3D]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Qué es la impresión 3D FDM (y diferencias con resina y SLS)]]
- [[Partes de una impresora FDM (CoreXY, extrusor, hotend y cama)]]
- [[Elegoo Centauri 2 Combo]]
- [[Sistema multicolor CANVAS de Elegoo]]
- [[Seguridad al imprimir (ventilación, emisiones y ubicación)]]

## Progreso

```base
filters:
  and:
    - file.inFolder("36-Impresion-3D/01 Fundamentos")
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
