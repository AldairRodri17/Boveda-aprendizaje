---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Laboratorios y práctica
tags: [indice, laboratorios-practica]
---
# Herramientas de pago con plan gratuito o prueba

Parte de: [[16-Laboratorios-Practica/_Índice Laboratorios y práctica|Laboratorios y práctica]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- Sentry: ver [[Sentry]]
- [[Grafana Cloud - plan gratuito]]
- [[New Relic - plan gratuito]]
- [[Datadog - prueba gratuita]]
- [[SonarCloud]]
- [[Snyk - plan gratuito]]
- [[Postman - plan gratuito]]
- [[JetBrains - licencias para estudiantes y proyectos de código abierto]]
- [[Cómo aprovechar una prueba gratuita sin olvidar cancelarla]]

## Progreso

```base
filters:
  and:
    - file.inFolder("16-Laboratorios-Practica/07 Herramientas de pago con plan gratuito o prueba")
    - file.name != "_Índice Herramientas de pago con plan gratuito o prueba"
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
