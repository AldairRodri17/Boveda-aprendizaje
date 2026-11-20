---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Power BI
tags: [indice, power-bi]
---
# Gráficas propias

Parte de: [[17-Power-BI/_Índice Power BI|Power BI]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Visuales personalizados - panorama de opciones]]
- [[Visuales de AppSource]]
- [[Visuales de R]]
- [[Visuales de Python]]
- [[Deneb (Vega y Vega-Lite)]]
- [[SVG dentro de medidas DAX]]
- [[SDK de visuales de Power BI (TypeScript y pbiviz)]]
- [[Crear un visual propio con D3]]
- [[Empaquetar, probar y publicar un visual propio]]

## Laboratorios

- [[Lab 9 - Visual propio con Deneb o SVG en Power BI]]

## Progreso

```base
filters:
  and:
    - file.inFolder("17-Power-BI/07 Gráficas propias")
    - file.name != "_Índice Gráficas propias"
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
