---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Legal básico personal
tags: [indice, legal]
---
# Consumidor y patrimonio

Parte de: [[48-Legal-Personal/_Índice Legal básico personal|Legal básico personal]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[PROFECO y tus derechos como consumidor]]
- [[Garantías y devoluciones]]
- [[Testamento y beneficiarios (Afore, seguros y cuentas)]]

## Progreso

```base
filters:
  and:
    - file.inFolder("48-Legal-Personal/04 Consumidor y patrimonio")
    - file.name != "_Índice Consumidor y patrimonio"
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
