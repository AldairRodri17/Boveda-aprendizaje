---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Hablar en público y exposiciones
tags: [indice, hablar-en-publico]
---
# Apoyos visuales

Parte de: [[40-Hablar-en-Publico/_Índice Hablar en público y exposiciones|Hablar en público y exposiciones]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Diapositivas efectivas (una idea por lámina)]] ⭐
- [[Mostrar datos y gráficas en una exposición]]
- [[Demos en vivo sin desastres]]
- [[Cuándo no usar diapositivas]]
- [[Notas del orador y control del tiempo]]

## Progreso

```base
filters:
  and:
    - file.inFolder("40-Hablar-en-Publico/04 Apoyos visuales")
    - file.name != "_Índice Apoyos visuales"
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
