---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Astronomía y astrofotografía
tags: [indice, astronomia]
---
# Astrofotografía avanzada

Parte de: [[64-Astronomia/_Índice Astronomía y astrofotografía|Astronomía y astrofotografía]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Montura con seguimiento (star tracker)]]
- [[Apilado de imágenes (Sequator, Siril y DeepSkyStacker)]] ⭐
- [[Tomas de calibración (darks, flats y bias)]]
- [[Fotografía lunar y planetaria]]
- [[Cielo profundo (nebulosas y galaxias)]]

## Laboratorios

- [[Lab 3 - Apilar 20 fotos en Sequator o Siril]]

## Progreso

```base
filters:
  and:
    - file.inFolder("64-Astronomia/04 Astrofotografía avanzada")
    - file.name != "_Índice Astrofotografía avanzada"
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
