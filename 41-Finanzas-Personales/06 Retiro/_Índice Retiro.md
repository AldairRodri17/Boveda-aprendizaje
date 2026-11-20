---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Salud financiera
tags: [indice, finanzas]
---
# Retiro

Parte de: [[41-Finanzas-Personales/_Índice Salud financiera|Salud financiera]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Afore - cómo funciona y cómo elegirla]] ⭐
- [[Aportaciones voluntarias a la Afore]] ⭐
- [[Plan Personal de Retiro (PPR)]]
- [[Cuánto necesitas para retirarte]]
- [[Semanas cotizadas y pensión del IMSS]]

## Laboratorios

- [[Lab 4 - Revisar mi Afore y simular mi retiro]]

## Progreso

```base
filters:
  and:
    - file.inFolder("41-Finanzas-Personales/06 Retiro")
    - file.name != "_Índice Retiro"
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
