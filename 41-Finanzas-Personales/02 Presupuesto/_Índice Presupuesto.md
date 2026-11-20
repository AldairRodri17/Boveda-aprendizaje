---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Salud financiera
tags: [indice, finanzas]
---
# Presupuesto

Parte de: [[41-Finanzas-Personales/_Índice Salud financiera|Salud financiera]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Presupuesto personal paso a paso]] ⭐
- [[Regla 50-30-20 y alternativas]]
- [[Gastos hormiga]]
- [[Herramientas para llevar tus finanzas (hoja de cálculo y apps)]]
- [[Finanzas en pareja o en familia]]

## Laboratorios

- [[Lab 1 - Mi presupuesto real de un mes]]

## Progreso

```base
filters:
  and:
    - file.inFolder("41-Finanzas-Personales/02 Presupuesto")
    - file.name != "_Índice Presupuesto"
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
