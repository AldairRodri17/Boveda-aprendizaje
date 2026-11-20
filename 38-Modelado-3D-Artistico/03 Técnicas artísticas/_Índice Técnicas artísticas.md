---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Modelado 3D artístico
tags: [indice, modelado-3d-artistico]
---
# Técnicas artísticas

Parte de: [[38-Modelado-3D-Artistico/_Índice Modelado 3D artístico|Modelado 3D artístico]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Proporciones y anatomía básica para figuras]]
- [[Cortar modelos en piezas y pines de unión]]
- [[Bustos y figuras originales]]
- [[Dioramas]]
- [[Litofanías]]
- [[Macetas, lámparas y objetos decorativos]]
- [[Modelos pensados para impresión multicolor]]
- [[Pintura de figuras impresas]]

## Laboratorios

- [[Lab 4 - Litofanía con una foto propia]]
- [[Lab 5 - Modelo original para 4 colores con CANVAS]]
- [[Lab 6 - Pintar una figura impresa (primer, base, lavado y pincel seco)]]

## Progreso

```base
filters:
  and:
    - file.inFolder("38-Modelado-3D-Artistico/03 Técnicas artísticas")
    - file.name != "_Índice Técnicas artísticas"
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
