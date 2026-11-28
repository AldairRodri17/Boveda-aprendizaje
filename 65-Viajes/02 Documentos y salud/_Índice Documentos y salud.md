---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Viajes y fotografía de viaje
tags: [indice, viajes]
contenido: completo
---
# Documentos y salud

Parte de: [[65-Viajes/_Índice Viajes y fotografía de viaje|Viajes y fotografía de viaje]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Pasaporte mexicano]] ⭐
- [[Visas (cómo saber si necesitas una)]] ⭐
- [[Seguro de viaje]]
- [[Salud en viaje (vacunas y botiquín)]]

## Progreso

```base
filters:
  and:
    - file.inFolder("65-Viajes/02 Documentos y salud")
    - file.name != "_Índice Documentos y salud"
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
