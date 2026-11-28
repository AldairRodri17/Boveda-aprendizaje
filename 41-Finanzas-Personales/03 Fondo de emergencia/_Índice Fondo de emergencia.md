---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Salud financiera
tags: [indice, finanzas]
contenido: completo
---
# Fondo de emergencia

Parte de: [[41-Finanzas-Personales/_Índice Salud financiera|Salud financiera]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Fondo de emergencia (cuánto y dónde tenerlo)]] ⭐

## Laboratorios

- [[Lab 2 - Plan para mi fondo de emergencia]]

## Progreso

```base
filters:
  and:
    - file.inFolder("41-Finanzas-Personales/03 Fondo de emergencia")
    - file.name != "_Índice Fondo de emergencia"
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
