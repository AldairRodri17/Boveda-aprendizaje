---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Edición de fotografía
tags: [indice, edicion-foto]
---
# Capas y composición

Parte de: [[32-Edicion-Foto/_Índice Edición de fotografía|Edición de fotografía]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Capas, máscaras y modos de fusión]]
- [[Selecciones y quitar fondos]]
- [[Fotomontaje]]

## Laboratorios

- [[Práctica Edición 07 - Quitar fondo y fotomontaje]]

## Progreso

```base
filters:
  and:
    - file.inFolder("32-Edicion-Foto/05 Capas y composición")
    - file.name != "_Índice Capas y composición"
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
