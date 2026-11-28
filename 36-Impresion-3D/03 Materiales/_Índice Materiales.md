---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Impresión 3D
tags: [indice, impresion-3d]
contenido: completo
---
# Materiales

Parte de: [[36-Impresion-3D/_Índice Impresión 3D|Impresión 3D]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[PLA]]
- [[PETG]]
- [[TPU (filamento flexible)]]
- [[Filamentos especiales (seda, mate, madera y brillo)]]
- [[Materiales que necesitan impresora cerrada (ABS, ASA, PC y nylon)]]
- [[Secado y almacenamiento de filamento]]

## Laboratorios

- [[Lab 6 - Secado de filamento casero vs secador]]

## Progreso

```base
filters:
  and:
    - file.inFolder("36-Impresion-3D/03 Materiales")
    - file.name != "_Índice Materiales"
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
