---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Ciberseguridad personal y vida digital
tags: [indice, ciberseguridad-personal]
contenido: completo
---
# Respaldo y vida digital

Parte de: [[51-Ciberseguridad-Personal/_Índice Ciberseguridad personal y vida digital|Ciberseguridad personal y vida digital]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Respaldo 3-2-1 de fotos y videos]] (vive en 30 · Fotografía)
- [[Respaldo de toda tu vida digital]] ⭐
- [[Nube vs disco externo]]
- [[Herencia digital (qué pasa con tus cuentas)]]

## Laboratorios

- [[Lab 2 - Plan de respaldo de mi vida digital]]

## Progreso

```base
filters:
  and:
    - file.inFolder("51-Ciberseguridad-Personal/04 Respaldo y vida digital")
    - file.name != "_Índice Respaldo y vida digital"
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
