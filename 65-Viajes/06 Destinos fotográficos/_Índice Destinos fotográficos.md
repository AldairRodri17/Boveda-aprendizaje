---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Viajes y fotografía de viaje
tags: [indice, viajes]
contenido: completo
---
# Destinos fotográficos

Parte de: [[65-Viajes/_Índice Viajes y fotografía de viaje|Viajes y fotografía de viaje]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Rutas fotográficas en México]] ⭐
- [[Pueblos Mágicos para fotografiar]]
- [[Zonas arqueológicas (reglas para foto, video y dron)]] ⭐
- [[Parques nacionales y naturaleza en México]]

## Progreso

```base
filters:
  and:
    - file.inFolder("65-Viajes/06 Destinos fotográficos")
    - file.name != "_Índice Destinos fotográficos"
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
