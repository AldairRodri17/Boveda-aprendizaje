---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Restauración del Vocho
tags: [indice, vocho]
contenido: completo
---
# Piezas y costos

Parte de: [[68-Restauracion-Vocho/_Índice Restauración del Vocho|Restauración del Vocho]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Dónde conseguir refacciones del vocho en México]] ⭐
- [[Piezas originales, réplica y usadas]]
- [[Controlar el costo de la restauración]] ⭐
- [[Clubes y comunidades de vocho]]

## Progreso

```base
filters:
  and:
    - file.inFolder("68-Restauracion-Vocho/09 Piezas y costos")
    - file.name != "_Índice Piezas y costos"
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
