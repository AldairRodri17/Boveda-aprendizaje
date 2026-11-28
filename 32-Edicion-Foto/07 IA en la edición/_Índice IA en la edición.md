---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Edición de fotografía
tags: [indice, edicion-foto]
contenido: completo
---
# IA en la edición

Parte de: [[32-Edicion-Foto/_Índice Edición de fotografía|Edición de fotografía]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Relleno generativo y quitar objetos]]
- [[Reducción de ruido con IA]]
- [[Ética y derechos de las imágenes con IA]]

## Laboratorios

- [[Práctica Edición 10 - Quitar objetos, manual vs IA]]

## Progreso

```base
filters:
  and:
    - file.inFolder("32-Edicion-Foto/07 IA en la edición")
    - file.name != "_Índice IA en la edición"
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
