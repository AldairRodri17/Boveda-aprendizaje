---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Integraciones
tags: [indice, integraciones]
contenido: completo
---
# Escritorio y hardware

Parte de: [[22-Integraciones/_Índice Integraciones|Integraciones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Electron]]
- [[Preload y contextBridge]]
- [[SDK BD Rowa y WWKS2]]

## Progreso

```base
filters:
  and:
    - file.inFolder("22-Integraciones/04 Escritorio y hardware")
    - file.name != "_Índice Escritorio y hardware"
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
