---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Inversiones
tags: [indice, inversiones]
contenido: completo
---
# Instrumentos de deuda

Parte de: [[42-Inversiones/_Índice Inversiones|Inversiones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Cetes]] ⭐
- [[Bonos gubernamentales (Bondes, Udibonos y Mbonos)]]
- [[Pagarés bancarios y cuentas con rendimiento]]
- [[Sofipos y protección de depósitos (IPAB y Prosofipo)]] ⭐

## Laboratorios

- [[Lab 2 - Comparar Cetes, pagaré y Sofipo con rendimiento real]]

## Progreso

```base
filters:
  and:
    - file.inFolder("42-Inversiones/02 Instrumentos de deuda")
    - file.name != "_Índice Instrumentos de deuda"
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
