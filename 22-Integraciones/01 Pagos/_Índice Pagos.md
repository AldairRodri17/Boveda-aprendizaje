---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Integraciones
tags: [indice, integraciones]
contenido: completo
---
# Pagos

Parte de: [[22-Integraciones/_Índice Integraciones|Integraciones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Pasarelas de pago - conceptos]]
- [[Cargos, intents y estados de un pago]]
- [[Pagos en efectivo y transferencia (OXXO y SPEI)]]
- [[Marketplaces y comisiones]]
- [[Conciliación de pagos]]
- [[PCI DSS]]
- [[Stripe]]
- [[Stripe Connect]]
- [[OpenPay]]

## Laboratorios

- [[Lab 1 - Pago de prueba con Stripe y webhook en .NET]]

## Progreso

```base
filters:
  and:
    - file.inFolder("22-Integraciones/01 Pagos")
    - file.name != "_Índice Pagos"
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
