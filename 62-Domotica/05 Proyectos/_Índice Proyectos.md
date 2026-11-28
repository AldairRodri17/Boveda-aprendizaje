---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Domótica y casa inteligente
tags: [indice, domotica]
---
# Proyectos

Parte de: [[62-Domotica/_Índice Domótica y casa inteligente|Domótica y casa inteligente]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Monitoreo de la impresora 3D desde Home Assistant]]
- [[Control de humedad para el filamento]]
- [[Alertas de consumo eléctrico]]
- [[Automatizar las luces del área de trabajo]]

## Progreso

```base
filters:
  and:
    - file.inFolder("62-Domotica/05 Proyectos")
    - file.name != "_Índice Proyectos"
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
