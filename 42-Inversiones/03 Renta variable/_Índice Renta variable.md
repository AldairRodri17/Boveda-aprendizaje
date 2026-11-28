---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Inversiones
tags: [indice, inversiones]
contenido: completo
---
# Renta variable

Parte de: [[42-Inversiones/_Índice Inversiones|Inversiones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Qué es una acción y cómo funciona la bolsa]]
- [[Fondos de inversión]]
- [[ETFs y fondos indexados]] ⭐
- [[SIC (Sistema Internacional de Cotizaciones)]]
- [[Dividendos]]

## Progreso

```base
filters:
  and:
    - file.inFolder("42-Inversiones/03 Renta variable")
    - file.name != "_Índice Renta variable"
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
