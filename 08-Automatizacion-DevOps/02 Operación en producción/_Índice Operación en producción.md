---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Automatización, DevOps y operación
tags: [indice, automatizacion-devops]
---
# Operación en producción

Parte de: [[08-Automatizacion-DevOps/_Índice Automatización, DevOps y operación|Automatización, DevOps y operación]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[SLI, SLO y SLA]]
  - [[SLI (indicador de nivel de servicio)]]
  - [[SLO (objetivo de nivel de servicio)]]
  - [[SLA (acuerdo de nivel de servicio)]]
- [[Error budgets]]
- [[Manejo de incidentes]]
- [[Postmortems sin culpables]]
- [[Runbooks]]
- [[Guardias (on-call)]]
- [[Recuperación ante desastres (RPO y RTO)]]
- [[Multi-región y alta disponibilidad]]
- [[Chaos engineering (conceptos)]]
- [[Pruebas de carga (k6 y NBomber)]]

## Laboratorios

- [[Lab 3 - Simulacro de incidente con SLO, error budget y postmortem]]
- [[Lab 4 - Prueba de carga con k6]]

## Progreso

```base
filters:
  and:
    - file.inFolder("08-Automatizacion-DevOps/02 Operación en producción")
    - file.name != "_Índice Operación en producción"
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
