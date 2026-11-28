---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Power BI
tags: [indice, power-bi]
contenido: completo
---
# Modelado de datos

Parte de: [[17-Power-BI/_Índice Power BI|Power BI]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Modelo estrella (hechos y dimensiones)]]
- [[Granularidad de una tabla de hechos]]
- [[Relaciones (cardinalidad y dirección del filtro)]]
- [[Relaciones activas e inactivas]]
- [[Relaciones muchos a muchos y bidireccionales]]
- [[Dimensiones de rol múltiple]]
- [[Tabla de calendario]]
- [[Jerarquías]]
- [[Esquema copo de nieve vs estrella]]
- [[Seguridad a nivel de fila (RLS)]]
- [[Seguridad a nivel de objeto (OLS)]]
- [[Cómo funciona el motor VertiPaq]]

## Laboratorios

- [[Lab 4 - Modelo estrella con tabla de calendario en Power BI]]
- [[Lab 5 - Seguridad a nivel de fila (RLS) en Power BI]]

## Progreso

```base
filters:
  and:
    - file.inFolder("17-Power-BI/03 Modelado de datos")
    - file.name != "_Índice Modelado de datos"
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
