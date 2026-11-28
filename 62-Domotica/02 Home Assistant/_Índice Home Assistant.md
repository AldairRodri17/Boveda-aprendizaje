---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Domótica y casa inteligente
tags: [indice, domotica]
contenido: completo
---
# Home Assistant

Parte de: [[62-Domotica/_Índice Domótica y casa inteligente|Domótica y casa inteligente]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Instalar Home Assistant]] ⭐
- [[Integraciones y dispositivos en Home Assistant]]
- [[Automatizaciones en Home Assistant]] ⭐
- [[Escenas y scripts en Home Assistant]]
- [[Dashboards de Home Assistant]]
- [[Complementos de Home Assistant]]

## Laboratorios

- [[Lab 1 - Home Assistant y primera automatización]]

## Progreso

```base
filters:
  and:
    - file.inFolder("62-Domotica/02 Home Assistant")
    - file.name != "_Índice Home Assistant"
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
