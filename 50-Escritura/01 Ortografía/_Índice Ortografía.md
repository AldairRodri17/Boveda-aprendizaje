---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Escritura clara
tags: [indice, escritura]
contenido: completo
---
# Ortografía

Parte de: [[50-Escritura/_Índice Escritura clara|Escritura clara]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Reglas de acentuación (agudas, graves y esdrújulas)]] ⭐
- [[Acento diacrítico (tú y tu, más y mas, sé y se)]] ⭐
- [[Diptongos, triptongos e hiatos]]
- [[Uso de b y v]]
- [[Uso de c, s y z]]
- [[Uso de g y j]]
- [[Uso de h]]
- [[Uso de ll e y]]
- [[Uso de r y rr]]
- [[Uso de mayúsculas]] ⭐
- [[Porque, porqué, por qué y por que]] ⭐
- [[Haber, a ver y a haber]]
- [[Hay, ahí y ay]]
- [[Errores ortográficos más comunes]] ⭐
- [[Abreviaturas, siglas y escritura de números]]

## Laboratorios

- [[Lab 1 - Dictado y examen de ortografía]]

## Progreso

```base
filters:
  and:
    - file.inFolder("50-Escritura/01 Ortografía")
    - file.name != "_Índice Ortografía"
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
