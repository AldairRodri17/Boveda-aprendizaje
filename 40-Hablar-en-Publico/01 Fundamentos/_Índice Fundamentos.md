---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Hablar en público y exposiciones
tags: [indice, hablar-en-publico]
---
# Fundamentos

Parte de: [[40-Hablar-en-Publico/_Índice Hablar en público y exposiciones|Hablar en público y exposiciones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Por qué cuesta hablar en público]] ⭐
- [[Miedo escénico y manejo de nervios]] ⭐
- [[Conocer a tu público]] ⭐
- [[Preparar una exposición paso a paso]] ⭐
- [[Ensayar con método]]

## Progreso

```base
filters:
  and:
    - file.inFolder("40-Hablar-en-Publico/01 Fundamentos")
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
