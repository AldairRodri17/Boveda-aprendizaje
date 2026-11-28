---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Viajes y fotografía de viaje
tags: [indice, viajes]
contenido: completo
---
# Planeación

Parte de: [[65-Viajes/_Índice Viajes y fotografía de viaje|Viajes y fotografía de viaje]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Planear un viaje paso a paso]] ⭐
- [[Presupuesto de viaje]]
- [[Vuelos y hospedaje (cómo encontrar mejores precios)]]
- [[Itinerario flexible]]

## Laboratorios

- [[Lab 1 - Plan completo de un viaje fotográfico de fin de semana]]

## Progreso

```base
filters:
  and:
    - file.inFolder("65-Viajes/01 Planeación")
    - file.name != "_Índice Planeación"
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
