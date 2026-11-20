---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Edición de video
tags: [indice, edicion-video]
---
# Montaje

Parte de: [[33-Edicion-Video/_Índice Edición de video|Edición de video]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Tipos de corte (J-cut, L-cut, jump cut y match cut)]]
  - [[J-cut y L-cut (edición)]]
  - [[Jump cut (edición)]]
  - [[Match cut (edición)]]
- [[Ritmo y narrativa en el montaje]]
- [[Editar al ritmo de la música]]
- [[Uso del b-roll en la edición]]

## Laboratorios

- [[Lab 2 - Montaje de entrevista con J-cuts y L-cuts]]
- [[Lab 3 - Reel vertical de 30 segundos editado a la música]]

## Progreso

```base
filters:
  and:
    - file.inFolder("33-Edicion-Video/02 Montaje")
    - file.name != "_Índice Montaje"
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
