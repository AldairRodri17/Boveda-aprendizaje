---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Certificaciones
tags: [indice, certificaciones]
---
# Power BI y datos

Parte de: [[18-Certificaciones/_Índice Certificaciones|Certificaciones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[PL-300 Power BI Data Analyst Associate]]
- [[DP-600 Fabric Analytics Engineer Associate]]
- [[DP-900 Azure Data Fundamentals]]

## Progreso

```base
filters:
  and:
    - file.inFolder("18-Certificaciones/03 Power BI y datos")
    - file.name != "_Índice Power BI y datos"
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
