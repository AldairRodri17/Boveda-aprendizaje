---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Idiomas
tags: [indice, idiomas]
---
# Habilidades

Parte de: [[49-Idiomas/_Índice Idiomas|Idiomas]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Comprensión auditiva en inglés (listening)]] ⭐
- [[Lectura en inglés]]
- [[Escritura en inglés]] ⭐
- [[Conversación en inglés (speaking)]] ⭐
- [[Pensar en inglés]]

## Laboratorios

- [[Lab 4 - Escribir un correo profesional en inglés y corregirlo]]

## Progreso

```base
filters:
  and:
    - file.inFolder("49-Idiomas/08 Habilidades")
    - file.name != "_Índice Habilidades"
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
