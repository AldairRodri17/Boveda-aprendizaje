---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Carrera y liderazgo técnico
tags: [indice, carrera-liderazgo]
contenido: completo
---
# Liderazgo y comunicación

Parte de: [[14-Carrera-Liderazgo/_Índice Carrera y liderazgo técnico|Carrera y liderazgo técnico]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Cómo hacer un buen code review]]
- [[Cómo documentar una decisión técnica]]
- [[Cómo explicar un tema técnico]]
- [[Comunicación con personas no técnicas]]
- [[Influir sin autoridad]]
- [[Negociar alcance y plazos con negocio]]
- [[Priorizar y saber decir que no]]
- [[Pensamiento de producto]]
- [[Dar y recibir feedback]]
- [[Manejo de conflictos]]
- [[Estimación de tareas]]
- [[Mentoría a otros desarrolladores]]
- [[Presentaciones técnicas]]
- [[Escribir artículos técnicos]]

## Laboratorios

- [[Lab 2 - Escribir un design doc corto]]

## Progreso

```base
filters:
  and:
    - file.inFolder("14-Carrera-Liderazgo/02 Liderazgo y comunicación")
    - file.name != "_Índice Liderazgo y comunicación"
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
