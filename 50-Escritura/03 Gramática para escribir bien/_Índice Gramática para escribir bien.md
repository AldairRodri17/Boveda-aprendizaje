---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Escritura clara
tags: [indice, escritura]
---
# Gramática para escribir bien

Parte de: [[50-Escritura/_Índice Escritura clara|Escritura clara]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Concordancia de género y número]] ⭐
- [[Dequeísmo y queísmo]]
- [[Uso correcto del gerundio]]
- [[Voz activa y voz pasiva en español]]
- [[Leísmo, laísmo y loísmo]]

## Progreso

```base
filters:
  and:
    - file.inFolder("50-Escritura/03 Gramática para escribir bien")
    - file.name != "_Índice Gramática para escribir bien"
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
