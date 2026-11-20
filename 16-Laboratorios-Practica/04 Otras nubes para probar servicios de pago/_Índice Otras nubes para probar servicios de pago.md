---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Laboratorios y práctica
tags: [indice, laboratorios-practica]
---
# Otras nubes para probar servicios de pago

Parte de: [[16-Laboratorios-Practica/_Índice Laboratorios y práctica|Laboratorios y práctica]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[AWS Free Tier - plan gratuito, créditos y servicios siempre gratuitos]]
- [[AWS - plan gratuito vs plan de pago (qué pasa cuando se acaban los créditos)]]
- [[Google Cloud - prueba gratuita (crédito por 90 días) y Free Tier]]
- [[Google Cloud Skills Boost (laboratorios con credenciales temporales)]]
- [[Google Cloud Shell]]
- [[Oracle Cloud - Always Free]]
- [[Comparativa de capas gratuitas de nubes (AWS, Azure, Google Cloud y Oracle)]]
- Cobros inesperados: ver [[Cómo evitar cobros inesperados en la nube (presupuestos y alertas)]]

## Progreso

```base
filters:
  and:
    - file.inFolder("16-Laboratorios-Practica/04 Otras nubes para probar servicios de pago")
    - file.name != "_Índice Otras nubes para probar servicios de pago"
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
