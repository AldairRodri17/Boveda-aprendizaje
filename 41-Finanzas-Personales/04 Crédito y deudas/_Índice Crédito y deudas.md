---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Salud financiera
tags: [indice, finanzas]
---
# Crédito y deudas

Parte de: [[41-Finanzas-Personales/_Índice Salud financiera|Salud financiera]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Cómo funciona una tarjeta de crédito (fecha de corte, fecha límite y pago)]] ⭐
- [[Pago mínimo vs pago para no generar intereses]] ⭐
- [[CAT, tasa de interés y comisiones]] ⭐
- [[Buró de Crédito y Círculo de Crédito]]
- [[Estrategias para salir de deudas (bola de nieve y avalancha)]]
- [[Créditos personales, de nómina y meses sin intereses]]
- [[Deuda buena y deuda mala]]

## Laboratorios

- [[Lab 3 - Revisar mi estado de cuenta y mi reporte de crédito]]

## Progreso

```base
filters:
  and:
    - file.inFolder("41-Finanzas-Personales/04 Crédito y deudas")
    - file.name != "_Índice Crédito y deudas"
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
