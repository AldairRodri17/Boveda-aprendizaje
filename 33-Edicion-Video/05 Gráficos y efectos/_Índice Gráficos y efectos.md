---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Edición de video
tags: [indice, edicion-video]
---
# Gráficos y efectos

Parte de: [[33-Edicion-Video/_Índice Edición de video|Edición de video]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Títulos y lower thirds]]
- [[Subtítulos (SRT)]]
- [[Keyframes y animación básica]]
- [[Transiciones con criterio]]
- [[Chroma key]]
- [[Máscaras y tracking en video]]
- [[Estabilización en posproducción (Gyroflow)]]
- [[Rampas de velocidad]]
- [[Motion graphics básico (Fusion)]]

## Laboratorios

- [[Lab 6 - Subtítulos SRT y lower third]]
- [[Lab 7 - Rampa de velocidad y estabilización]]

## Progreso

```base
filters:
  and:
    - file.inFolder("33-Edicion-Video/05 Gráficos y efectos")
    - file.name != "_Índice Gráficos y efectos"
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
