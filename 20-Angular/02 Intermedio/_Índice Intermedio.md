---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Angular
tags: [indice, angular]
---
# Intermedio

Parte de: [[20-Angular/_Índice Angular|Angular]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Servicios e inyección de dependencias en Angular]]
- [[Signals]]
- [[computed y effect]]
- [[RxJS - Observables]]
- [[RxJS - operadores más usados]]
- [[Signals vs RxJS]]
- [[HttpClient en Angular]]
- [[Interceptores HTTP]]
- [[Router de Angular]]
- [[Guards y resolvers]]
- [[Lazy loading de rutas]]
- [[Formularios template-driven]]
- [[Formularios reactivos]]
- [[Validaciones personalizadas]]
- [[Manejo de errores en Angular]]

## Laboratorios

- [[Lab 1 - Signals en Angular (carrito de compras)]]
- [[Lab 2 - Servicio, HttpClient e interceptor en Angular]]
- [[Lab 3 - Router con guards y lazy loading en Angular]]
- [[Lab 4 - Formularios reactivos con validación en Angular]]

## Progreso

```base
filters:
  and:
    - file.inFolder("20-Angular/02 Intermedio")
    - file.name != "_Índice Intermedio"
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
