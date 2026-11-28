---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Restauración del Vocho
tags: [indice, vocho]
contenido: completo
---
# Transmisión y frenos

Parte de: [[68-Restauracion-Vocho/_Índice Restauración del Vocho|Restauración del Vocho]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Clutch y su ajuste]]
- [[Transeje y cambios]]
- [[Frenos de tambor y de disco]] ⭐
- [[Líquido de frenos y purgado]]

## Progreso

```base
filters:
  and:
    - file.inFolder("68-Restauracion-Vocho/04 Transmisión y frenos")
    - file.name != "_Índice Transmisión y frenos"
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
