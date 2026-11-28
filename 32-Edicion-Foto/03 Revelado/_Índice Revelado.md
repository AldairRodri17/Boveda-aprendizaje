---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Edición de fotografía
tags: [indice, edicion-foto]
contenido: completo
---
# Revelado

Parte de: [[32-Edicion-Foto/_Índice Edición de fotografía|Edición de fotografía]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Exposición, blancos y negros en el revelado]]
- [[Curvas de tonos]]
- [[HSL y gradación de color]]
- [[Nitidez y reducción de ruido]]
- [[Corrección de lente y perspectiva]]
- [[Recorte y enderezado]]

## Laboratorios

- [[Práctica Edición 04 - Revelado completo paso a paso]]

## Progreso

```base
filters:
  and:
    - file.inFolder("32-Edicion-Foto/03 Revelado")
    - file.name != "_Índice Revelado"
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
