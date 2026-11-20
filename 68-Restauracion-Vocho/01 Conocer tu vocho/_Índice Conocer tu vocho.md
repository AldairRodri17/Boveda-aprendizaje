---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Restauración del Vocho
tags: [indice, vocho]
---
# Conocer tu vocho

Parte de: [[68-Restauracion-Vocho/_Índice Restauración del Vocho|Restauración del Vocho]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Historia del Volkswagen Sedán en México]]
- [[Identificar año y versión de tu vocho (número de serie)]] ⭐
- [[Partes principales del vocho]] ⭐
- [[Evaluar el estado del vocho antes de restaurar]] ⭐
- [[Plan y presupuesto de restauración]] ⭐
- [[Restauración original vs personalizada]]

## Laboratorios

- [[Lab 1 - Inspección completa y lista de trabajos]]

## Progreso

```base
filters:
  and:
    - file.inFolder("68-Restauracion-Vocho/01 Conocer tu vocho")
    - file.name != "_Índice Conocer tu vocho"
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
