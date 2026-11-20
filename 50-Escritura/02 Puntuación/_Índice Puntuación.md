---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Escritura clara
tags: [indice, escritura]
---
# Puntuación

Parte de: [[50-Escritura/_Índice Escritura clara|Escritura clara]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[La coma]] ⭐
- [[Punto y seguido, punto y aparte y punto final]]
- [[Punto y coma y dos puntos]]
- [[Signos de interrogación y exclamación]]
- [[Comillas, paréntesis y rayas]]

## Progreso

```base
filters:
  and:
    - file.inFolder("50-Escritura/02 Puntuación")
    - file.name != "_Índice Puntuación"
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
