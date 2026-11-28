---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Legal básico personal
tags: [indice, legal]
---
# Contratos

Parte de: [[48-Legal-Personal/_Índice Legal básico personal|Legal básico personal]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Cómo leer un contrato antes de firmar]] ⭐
- [[Contrato de arrendamiento]]
- [[Contrato de servicios profesionales]]
- [[NDA y cláusulas de no competencia]]

## Progreso

```base
filters:
  and:
    - file.inFolder("48-Legal-Personal/02 Contratos")
    - file.name != "_Índice Contratos"
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
