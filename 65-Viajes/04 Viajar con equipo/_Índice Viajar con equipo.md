---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Viajes y fotografía de viaje
tags: [indice, viajes]
contenido: completo
---
# Viajar con equipo

Parte de: [[65-Viajes/_Índice Viajes y fotografía de viaje|Viajes y fotografía de viaje]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Viajar con cámara, lentes y dron]] ⭐
- [[Baterías en el avión]]
- [[Volar dron en otros países]] ⭐
- [[Respaldar fotos durante el viaje]] ⭐

## Progreso

```base
filters:
  and:
    - file.inFolder("65-Viajes/04 Viajar con equipo")
    - file.name != "_Índice Viajar con equipo"
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
