---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Power BI
tags: [indice, power-bi]
contenido: completo
---
# Power Query y el lenguaje M

Parte de: [[17-Power-BI/_Índice Power BI|Power BI]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Power Query - interfaz y pasos aplicados]]
- [[Power Query - tipos de datos]]
- [[Power Query - limpieza de datos]]
- [[Power Query - columnas condicionales y personalizadas]]
- [[Power Query - combinar consultas (merge y append)]]
- [[Power Query - agrupar, pivotar y despivotar]]
- [[Power Query - parámetros]]
- [[Power Query - funciones personalizadas]]
- [[Power Query - manejo de errores]]
- [[Power Query - consumir un API]]
- [[Power Query en Excel (consumir un API desde Excel)]]
- [[Lenguaje M - let e in]]
- [[Lenguaje M - tipos (listas, registros y tablas)]]
  - [[Listas en M]]
  - [[Registros en M]]
  - [[Tablas en M]]
- [[Lenguaje M - funciones y recursividad]]
- [[Query folding]]
- [[Actualización incremental]]
- [[Dataflows]]
- [[Consultas nativas de SQL y vistas para Power BI]]

## Laboratorios

- [[Lab 2 - Limpieza y combinación de datos en Power Query]]
- [[Lab 3 - Consumir un API en Power Query]]

## Progreso

```base
filters:
  and:
    - file.inFolder("17-Power-BI/02 Power Query y el lenguaje M")
    - file.name != "_Índice Power Query y el lenguaje M"
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
