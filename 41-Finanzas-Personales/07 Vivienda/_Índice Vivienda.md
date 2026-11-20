---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Salud financiera
tags: [indice, finanzas]
---
# Vivienda

Parte de: [[41-Finanzas-Personales/_Índice Salud financiera|Salud financiera]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Rentar vs comprar]] ⭐
- [[Crédito Infonavit]]
- [[Créditos hipotecarios bancarios]]
- [[Gastos al comprar una casa (escrituración, avalúo e impuestos)]]

## Progreso

```base
filters:
  and:
    - file.inFolder("41-Finanzas-Personales/07 Vivienda")
    - file.name != "_Índice Vivienda"
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
