---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Hablar en público y exposiciones
tags: [indice, hablar-en-publico]
---
# Estructura del mensaje

Parte de: [[40-Hablar-en-Publico/_Índice Hablar en público y exposiciones|Hablar en público y exposiciones]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Mensaje central en una frase]] ⭐
- [[Aperturas que enganchan]] ⭐
- [[Estructuras de charla (problema-solución, pasado-presente-futuro y regla de tres)]]
- [[Storytelling para exposiciones]] ⭐
- [[Cierres y llamada a la acción]] ⭐
- [[Transiciones entre ideas]]

## Laboratorios

- [[Lab 1 - Pitch de 2 minutos de un proyecto propio]]

## Progreso

```base
filters:
  and:
    - file.inFolder("40-Hablar-en-Publico/02 Estructura del mensaje")
    - file.name != "_Índice Estructura del mensaje"
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
