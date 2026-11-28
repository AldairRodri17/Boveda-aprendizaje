---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Impresión 3D
tags: [indice, impresion-3d]
contenido: completo
---
# Posprocesado

Parte de: [[36-Impresion-3D/_Índice Impresión 3D|Impresión 3D]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Quitar soportes y limpiar piezas]]
- [[Lijado y relleno de piezas impresas]]
- [[Primer, pintura y barniz para piezas impresas]]
- [[Insertos térmicos, tornillería y pegado]]

## Laboratorios

- [[Lab 7 - Posprocesado soportes, lijado, primer y pintura]]
- [[Lab 8 - Inserto térmico con cautín]]

## Progreso

```base
filters:
  and:
    - file.inFolder("36-Impresion-3D/05 Posprocesado")
    - file.name != "_Índice Posprocesado"
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
