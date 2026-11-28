---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Inversiones
tags: [indice, inversiones]
contenido: completo
---
# Otros activos

Parte de: [[42-Inversiones/_Índice Inversiones|Inversiones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[FIBRAs]]
- [[Bienes raíces como inversión]]
- [[Oro y materias primas]]
- [[Criptomonedas (riesgos y realidad)]]

## Progreso

```base
filters:
  and:
    - file.inFolder("42-Inversiones/04 Otros activos")
    - file.name != "_Índice Otros activos"
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
