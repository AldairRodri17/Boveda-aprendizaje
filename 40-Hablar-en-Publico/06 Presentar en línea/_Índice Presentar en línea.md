---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Hablar en público y exposiciones
tags: [indice, hablar-en-publico]
contenido: completo
---
# Presentar en línea

Parte de: [[40-Hablar-en-Publico/_Índice Hablar en público y exposiciones|Hablar en público y exposiciones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Cámara, luz y audio para presentar en línea]] (enlaza con 31 y 34)
- [[Presentar en Teams y Zoom]]
- [[Mantener la atención en reuniones virtuales]]

## Progreso

```base
filters:
  and:
    - file.inFolder("40-Hablar-en-Publico/06 Presentar en línea")
    - file.name != "_Índice Presentar en línea"
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
