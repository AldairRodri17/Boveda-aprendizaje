---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Escritura clara
tags: [indice, escritura]
---
# Caligrafía

Parte de: [[50-Escritura/_Índice Escritura clara|Escritura clara]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Postura, agarre del lápiz y materiales]] ⭐
- [[Letra de molde legible]] ⭐
- [[Letra cursiva]] ⭐
- [[Ritmo, inclinación y espaciado de la letra]]
- [[Rutina diaria de caligrafía]] ⭐
- [[Rúbrica de caligrafía]] ⭐ _(existe)_
- [[Tomar notas a mano]]
- [[Lettering y caligrafía creativa (opcional)]]

## Laboratorios

- [[Lab 2 - Plana de caligrafía de 15 minutos con foto]]

## Progreso

```base
filters:
  and:
    - file.inFolder("50-Escritura/04 Caligrafía")
    - file.name != "_Índice Caligrafía"
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
