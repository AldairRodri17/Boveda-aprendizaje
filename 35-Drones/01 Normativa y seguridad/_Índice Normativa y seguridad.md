---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Drones
tags: [indice, drones]
contenido: completo
---
# Normativa y seguridad

Parte de: [[35-Drones/_Índice Drones|Drones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[NOM-107-SCT3-2019 y registro ante la AFAC]]
- [[Categorías de uso de drones (recreativo, privado y comercial)]]
- [[Zonas restringidas y privacidad al volar]]
- [[Revisión antes de despegar (checklist)]]
- [[Viento, clima y retorno automático (RTH)]]
- [[Baterías de dron (cuidado y viajes)]]

## Laboratorios

- [[Lab 1 - Checklist pre-vuelo y revisión legal del lugar]]

## Progreso

```base
filters:
  and:
    - file.inFolder("35-Drones/01 Normativa y seguridad")
    - file.name != "_Índice Normativa y seguridad"
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
