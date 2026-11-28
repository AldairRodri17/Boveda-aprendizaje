---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Hablar en público y exposiciones
tags: [indice, hablar-en-publico]
contenido: completo
---
# Preguntas y respuestas

Parte de: [[40-Hablar-en-Publico/_Índice Hablar en público y exposiciones|Hablar en público y exposiciones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Manejar la ronda de preguntas]] ⭐
- [[Preguntas difíciles o agresivas]]
- [[Decir no sé con seguridad]]
- [[Interrupciones y público difícil]]

## Progreso

```base
filters:
  and:
    - file.inFolder("40-Hablar-en-Publico/07 Preguntas y respuestas")
    - file.name != "_Índice Preguntas y respuestas"
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
