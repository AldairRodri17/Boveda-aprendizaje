---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Fotografía
tags: [indice, fotografia]
contenido: completo
---
# Exposición

Parte de: [[30-Fotografia/_Índice Fotografía|Fotografía]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Triángulo de exposición]]
- [[Apertura y profundidad de campo]]
- [[Velocidad de obturación y movimiento]]
- [[ISO y ruido digital]]
- [[Medición de luz (matricial, ponderada y puntual)]]
- [[Compensación de exposición]]
- [[Histograma y rango dinámico]]
- [[Bracketing y HDR en fotografía]]

## Laboratorios

- [[Práctica Foto 02 - Velocidad, ISO y profundidad de campo]]
- [[Práctica Foto 03 - Medición, compensación y bracketing]]

## Progreso

```base
filters:
  and:
    - file.inFolder("30-Fotografia/02 Exposición")
    - file.name != "_Índice Exposición"
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
