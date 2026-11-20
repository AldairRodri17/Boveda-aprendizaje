---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Fotografía
tags: [indice, fotografia]
---
# Géneros

Parte de: [[30-Fotografia/_Índice Fotografía|Fotografía]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Fotografía de retrato]]
- [[Fotografía de paisaje]]
- [[Fotografía de calle]]
- [[Fotografía de arquitectura]]
- [[Fotografía de producto y comida]]
- [[Fotografía macro]]
- [[Fotografía nocturna y astrofotografía]]
- [[Larga exposición]]
- [[Fotografía de deporte y acción]]
- [[Fotografía de eventos]]
- [[Fotografía de viaje]]
- [[Fotografía de fauna]]

## Laboratorios

- [[Práctica Foto 08 - Larga exposición nocturna con el iPhone]]
- [[Práctica Foto 09 - Producto y comida en casa]]

## Progreso

```base
filters:
  and:
    - file.inFolder("30-Fotografia/06 Géneros")
    - file.name != "_Índice Géneros"
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
