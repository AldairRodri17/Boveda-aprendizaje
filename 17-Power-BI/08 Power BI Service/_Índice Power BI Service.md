---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Power BI
tags: [indice, power-bi]
---
# Power BI Service

Parte de: [[17-Power-BI/_Índice Power BI|Power BI]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Áreas de trabajo (workspaces)]]
- [[Modelos semánticos (antes datasets)]]
- [[Publicar y compartir informes]]
- [[Aplicaciones de Power BI]]
- [[Actualización programada]]
- [[Puerta de enlace de datos local (gateway)]]
- [[Paneles (dashboards) vs informes]]
- [[Suscripciones y alertas]]
- [[Seguridad y permisos en el Service]]
- [[Certificación y promoción de contenido]]
- [[Linaje de datos]]

## Progreso

```base
filters:
  and:
    - file.inFolder("17-Power-BI/08 Power BI Service")
    - file.name != "_Índice Power BI Service"
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
