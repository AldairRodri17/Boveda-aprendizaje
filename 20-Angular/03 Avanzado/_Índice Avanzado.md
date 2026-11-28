---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Angular
tags: [indice, angular]
contenido: completo
---
# Avanzado

Parte de: [[20-Angular/_Índice Angular|Angular]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Detección de cambios y zoneless]]
- [[OnPush y rendimiento]]
- [[Estado global (NgRx y Signal Store)]]
- [[SSR e hidratación en Angular]]
- [[Deferrable views (@defer)]]
- [[Angular Material y CDK]]
- [[Internacionalización en Angular]]
- [[Pruebas en Angular (unitarias y de componentes)]]
- [[Estructura de carpetas por feature en Angular]]
- [[Angular con un API .NET (autenticación y CORS)]]
- [[Proyecto práctico en Angular]]

## Laboratorios

- [[Lab 1 - Pruebas unitarias y de componentes en Angular]]

## Progreso

```base
filters:
  and:
    - file.inFolder("20-Angular/03 Avanzado")
    - file.name != "_Índice Avanzado"
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
