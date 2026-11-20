---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Fundamentos
tags: [indice, fundamentos]
---
# Git

Parte de: [[02-Fundamentos/_Índice Fundamentos|Fundamentos]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Git - qué es y cómo funciona]]
- [[Git - commit]]
- [[Git - ramas]]
- [[Git - merge]]
- [[Git - rebase]]
- [[Git - cherry-pick]]
- [[Git - stash]]
- [[Git - resolver conflictos]]
- [[Git - reset y revert]]
- [[Git - .gitignore]]
- [[Git - pull requests]]
- [[Estrategias de ramas (GitFlow y trunk-based)]]
- [[Conventional Commits]]
- [[Git - tags y versionado semántico]]
- [[Git - hooks y pre-commit]]

## Laboratorios

- [[Lab 1 - Ramas, merge, rebase y conflictos en Git]]

## Progreso

```base
filters:
  and:
    - file.inFolder("02-Fundamentos/07 Git")
    - file.name != "_Índice Git"
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
