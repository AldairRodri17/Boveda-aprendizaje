---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Gestión de personas
tags: [indice, gestion-personas]
---
# Desempeño

Parte de: [[53-Gestion-de-Personas/_Índice Gestión de personas|Gestión de personas]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Definir expectativas claras]] ⭐
- [[Feedback continuo (modelo SBI)]] ⭐
- [[Evaluaciones de desempeño]]
- [[Manejar bajo rendimiento]] ⭐
- [[Reconocimiento]]

## Laboratorios

- [[Lab 2 - Dar feedback SBI a un caso]]

## Progreso

```base
filters:
  and:
    - file.inFolder("53-Gestion-de-Personas/04 Desempeño")
    - file.name != "_Índice Desempeño"
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
