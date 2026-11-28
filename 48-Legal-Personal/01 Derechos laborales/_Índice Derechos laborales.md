---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Legal básico personal
tags: [indice, legal]
contenido: completo
---
# Derechos laborales

Parte de: [[48-Legal-Personal/_Índice Legal básico personal|Legal básico personal]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Ley Federal del Trabajo - lo esencial]] ⭐
- [[Contrato laboral (tipos y qué revisar)]] ⭐
- [[Aguinaldo, vacaciones y prima vacacional]] ⭐
- [[PTU (reparto de utilidades)]]
- [[Finiquito vs liquidación]] ⭐
- [[Subcontratación y outsourcing]]
- [[Teletrabajo (derechos y obligaciones)]]
- [[Renuncia y despido]]

## Laboratorios

- [[Lab 1 - Revisar mi contrato laboral con checklist]]
- [[Lab 2 - Calcular aguinaldo, vacaciones y finiquito]]

## Progreso

```base
filters:
  and:
    - file.inFolder("48-Legal-Personal/01 Derechos laborales")
    - file.name != "_Índice Derechos laborales"
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
