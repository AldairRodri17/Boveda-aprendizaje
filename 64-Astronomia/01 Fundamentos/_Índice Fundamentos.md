---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Astronomía y astrofotografía
tags: [indice, astronomia]
---
# Fundamentos

Parte de: [[64-Astronomia/_Índice Astronomía y astrofotografía|Astronomía y astrofotografía]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[El cielo nocturno (constelaciones, planetas y estrellas)]] ⭐
- [[Movimiento del cielo (rotación, estaciones y fases de la Luna)]] ⭐
- [[Apps y mapas del cielo (Stellarium y similares)]] ⭐
- [[Contaminación lumínica y escala de Bortle]] ⭐
- [[Cielos oscuros en México]]

## Laboratorios

- [[Lab 1 - Identificar 5 constelaciones con Stellarium]]

## Progreso

```base
filters:
  and:
    - file.inFolder("64-Astronomia/01 Fundamentos")
    - file.name != "_Índice Fundamentos"
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
