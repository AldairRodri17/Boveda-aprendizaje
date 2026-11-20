---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Restauración del Vocho
tags: [indice, vocho]
---
# Motor

Parte de: [[68-Restauracion-Vocho/_Índice Restauración del Vocho|Restauración del Vocho]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Cómo funciona un motor bóxer enfriado por aire]] ⭐
- [[Cambio de aceite y filtro del vocho]] ⭐
- [[Afinación del vocho (bujías, encendido y tiempo)]] ⭐
- [[Carburador o inyección (según el año)]]
- [[Enfriamiento por aire (turbina y termostato)]]
- [[Fallas comunes del motor y diagnóstico]]
- [[Reparar o reconstruir el motor (cuándo y con quién)]]

## Laboratorios

- [[Lab 2 - Cambio de aceite]]
- [[Lab 3 - Afinación básica]]

## Progreso

```base
filters:
  and:
    - file.inFolder("68-Restauracion-Vocho/03 Motor")
    - file.name != "_Índice Motor"
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
