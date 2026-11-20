---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Inversiones
tags: [indice, inversiones]
---
# Práctica

Parte de: [[42-Inversiones/_Índice Inversiones|Inversiones]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Cómo elegir casa de bolsa o app de inversión]] ⭐
- [[Comisiones y su efecto en el rendimiento]]
- [[Impuestos de las inversiones en México]]
- [[Leer un estado de cuenta de inversión]]
- [[Practicar con simulador antes de usar dinero real]]

## Laboratorios

- [[Lab 4 - Analizar las comisiones de una plataforma]]

## Progreso

```base
filters:
  and:
    - file.inFolder("42-Inversiones/06 Práctica")
    - file.name != "_Índice Práctica"
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
