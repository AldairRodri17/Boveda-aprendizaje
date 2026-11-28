---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Salud financiera
tags: [indice, finanzas]
---
# Fundamentos

Parte de: [[41-Finanzas-Personales/_Índice Salud financiera|Salud financiera]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Qué es la salud financiera]] ⭐
- [[Ingreso, gasto, ahorro e inversión]]
- [[Metas financieras (corto, mediano y largo plazo)]] ⭐
- [[Patrimonio neto (cómo calcularlo)]]

## Progreso

```base
filters:
  and:
    - file.inFolder("41-Finanzas-Personales/01 Fundamentos")
    - file.name != "_Índice Fundamentos"
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
