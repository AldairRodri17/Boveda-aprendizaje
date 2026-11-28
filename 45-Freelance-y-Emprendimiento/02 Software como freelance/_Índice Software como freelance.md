---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Freelance y emprendimiento
tags: [indice, freelance]
contenido: completo
---
# Software como freelance

Parte de: [[45-Freelance-y-Emprendimiento/_Índice Freelance y emprendimiento|Freelance y emprendimiento]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Cotizar un proyecto de software]] ⭐
- [[Precio por hora vs por proyecto vs por valor]] ⭐
- [[Contratos, anticipos y alcance]] ⭐
- [[Manejar cambios de alcance]]
- [[Mantenimiento y soporte como ingreso recurrente]]
- [[Conseguir clientes]]

## Laboratorios

- [[Lab 1 - Cotización de un proyecto real]]

## Progreso

```base
filters:
  and:
    - file.inFolder("45-Freelance-y-Emprendimiento/02 Software como freelance")
    - file.name != "_Índice Software como freelance"
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
