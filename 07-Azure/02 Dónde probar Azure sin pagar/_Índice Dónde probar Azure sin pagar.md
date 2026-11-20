---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Azure
tags: [indice, azure]
---
# Dónde probar Azure sin pagar

Parte de: [[07-Azure/_Índice Azure|Azure]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Cuenta gratuita de Azure (crédito de 30 días, servicios gratuitos y límite de gasto)]]
- [[Servicios de Azure siempre gratuitos]]
- [[Servicios de Azure gratuitos durante 12 meses]]
- [[Probar servicios de pago con el crédito de la cuenta gratuita]]
- [[Azure for Students]]
- [[Microsoft Learn sandbox (suscripción temporal sin tarjeta)]]
- [[Créditos de Azure para suscriptores de Visual Studio]]
- [[Pay as you go y cuándo se pierde la protección de gasto]]
- [[Cómo evitar cobros inesperados en la nube (presupuestos y alertas)]]
- [[Cómo borrar recursos al terminar una práctica]]
- [[Crédito promocional vs proyectos con costo cero permanente]]

## Progreso

```base
filters:
  and:
    - file.inFolder("07-Azure/02 Dónde probar Azure sin pagar")
    - file.name != "_Índice Dónde probar Azure sin pagar"
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
