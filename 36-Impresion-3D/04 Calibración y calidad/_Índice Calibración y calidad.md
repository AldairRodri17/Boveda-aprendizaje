---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Impresión 3D
tags: [indice, impresion-3d]
---
# Calibración y calidad

Parte de: [[36-Impresion-3D/_Índice Impresión 3D|Impresión 3D]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Primera capa y calibración automática]]
- [[Calibración de flujo y pressure advance]]
- [[Torre de temperatura]]
- [[Calibración de retracción]]
- [[Prueba de tolerancias]]
- [[Problemas comunes de impresión FDM]]
  - [[Warping (esquinas levantadas) en FDM]]
  - [[Stringing (hilos) en FDM]]
  - [[Subextrusión y sobreextrusión en FDM]]
  - [[Pata de elefante en FDM]]
  - [[Desplazamiento de capas en FDM]]
  - [[Atascos de boquilla en FDM]]
- [[Mantenimiento de la impresora 3D]]

## Laboratorios

- [[Lab 1 - Primera impresión y revisión de calibración automática en Centauri 2]]
- [[Lab 2 - Torre de temperatura de PLA]]
- [[Lab 3 - Prueba de flujo y pressure advance]]
- [[Lab 4 - Prueba de tolerancias con pieza que encaja]]

## Progreso

```base
filters:
  and:
    - file.inFolder("36-Impresion-3D/04 Calibración y calidad")
    - file.name != "_Índice Calibración y calidad"
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
