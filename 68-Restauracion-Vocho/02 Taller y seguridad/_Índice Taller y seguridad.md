---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Restauración del Vocho
tags: [indice, vocho]
---
# Taller y seguridad

Parte de: [[68-Restauracion-Vocho/_Índice Restauración del Vocho|Restauración del Vocho]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Herramientas básicas de mecánica]] ⭐
- [[Seguridad al trabajar bajo un auto (gato y torres)]] ⭐
- [[Organizar el espacio y las piezas al desarmar]]
- [[Manuales de taller del vocho]]
- [[Documentar la restauración con fotos y bitácora]]

## Progreso

```base
filters:
  and:
    - file.inFolder("68-Restauracion-Vocho/02 Taller y seguridad")
    - file.name != "_Índice Taller y seguridad"
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
