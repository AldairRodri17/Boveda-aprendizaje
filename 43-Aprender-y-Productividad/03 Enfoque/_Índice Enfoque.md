---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Aprender a aprender y productividad
tags: [indice, productividad]
contenido: completo
---
# Enfoque

Parte de: [[43-Aprender-y-Productividad/_Índice Aprender a aprender y productividad|Aprender a aprender y productividad]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Trabajo profundo (deep work)]] ⭐
- [[Manejar distracciones y notificaciones]]
- [[El costo de la multitarea]]

## Progreso

```base
filters:
  and:
    - file.inFolder("43-Aprender-y-Productividad/03 Enfoque")
    - file.name != "_Índice Enfoque"
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
