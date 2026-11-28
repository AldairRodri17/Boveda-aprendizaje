---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Legal básico personal
tags: [indice, legal]
---
# Propiedad intelectual

Parte de: [[48-Legal-Personal/_Índice Legal básico personal|Legal básico personal]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Derechos de autor en México (INDAUTOR)]] ⭐
- [[Quién es dueño del código que escribes en tu trabajo]] ⭐
- [[Registro de marca (IMPI)]]
- [[Licencias de lo que creas]] (enlaza con 09 y 36)

## Progreso

```base
filters:
  and:
    - file.inFolder("48-Legal-Personal/03 Propiedad intelectual")
    - file.name != "_Índice Propiedad intelectual"
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
