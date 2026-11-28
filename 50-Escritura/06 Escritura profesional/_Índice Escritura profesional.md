---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Escritura clara
tags: [indice, escritura]
---
# Escritura profesional

Parte de: [[50-Escritura/_Índice Escritura clara|Escritura clara]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Correos profesionales]] ⭐
- [[Mensajes de trabajo en chat (Teams y Slack)]]
- [[Resumen ejecutivo por escrito]] ⭐
- [[Propuestas y cotizaciones por escrito]]
- [[Minutas y seguimiento de reuniones]]
- [[Documentación técnica]] (vive en 13)

## Laboratorios

- [[Lab 3 - Reescribir un correo real para que sea claro]]
- [[Lab 4 - Resumen ejecutivo de una página]]

## Progreso

```base
filters:
  and:
    - file.inFolder("50-Escritura/06 Escritura profesional")
    - file.name != "_Índice Escritura profesional"
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
