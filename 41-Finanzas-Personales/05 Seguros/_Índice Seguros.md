---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Salud financiera
tags: [indice, finanzas]
---
# Seguros

Parte de: [[41-Finanzas-Personales/_Índice Salud financiera|Salud financiera]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Seguro de gastos médicos mayores]] ⭐
- [[Seguro de vida]]
- [[Seguro de auto]]
- [[Seguro de hogar y de equipo (fotografía y dron)]]
- [[Cómo leer una póliza (suma asegurada, deducible y coaseguro)]]

## Progreso

```base
filters:
  and:
    - file.inFolder("41-Finanzas-Personales/05 Seguros")
    - file.name != "_Índice Seguros"
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
