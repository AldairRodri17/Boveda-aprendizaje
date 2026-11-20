---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Equipo
tags: [indice, equipo]
---
# Certificaciones opcionales

Parte de: [[34-Equipo/_Índice Equipo|Equipo]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Certificación de DaVinci Resolve (Blackmagic)]]
- [[Adobe Certified Professional (Photoshop y Premiere)]]

## Progreso

```base
filters:
  and:
    - file.inFolder("34-Equipo/04 Certificaciones opcionales")
    - file.name != "_Índice Certificaciones opcionales"
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
