---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Edición de fotografía
tags: [indice, edicion-foto]
---
# Programas gratuitos

Parte de: [[32-Edicion-Foto/_Índice Edición de fotografía|Edición de fotografía]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[darktable y Ansel]]
- [[RawTherapee]]
- [[GIMP]]
- [[Photopea]]
- [[Snapseed]]
- [[Lightroom móvil (versión gratuita)]]

## Laboratorios

- [[Práctica Edición 02 - Primer revelado de un ProRAW en darktable]]
- [[Práctica Edición 03 - El mismo archivo en cinco programas]]

## Progreso

```base
filters:
  and:
    - file.inFolder("32-Edicion-Foto/02 Programas gratuitos")
    - file.name != "_Índice Programas gratuitos"
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
