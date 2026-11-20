---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Freelance y emprendimiento
tags: [indice, freelance]
---
# Monetizar hobbies

Parte de: [[45-Freelance-y-Emprendimiento/_Índice Freelance y emprendimiento|Freelance y emprendimiento]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Vender fotografías (bancos de imágenes y clientes directos)]]
- [[Servicios de foto y video para eventos y negocios]]
- [[Servicios con dron (uso comercial y permisos)]] ⭐
- [[Vender impresiones 3D (costeo y licencias)]] ⭐
- [[Productos digitales (presets, LUTs y modelos 3D)]]

## Laboratorios

- [[Lab 2 - Precio de venta de una impresión 3D]]
- [[Lab 3 - Lean Canvas de un servicio con mis hobbies]]

## Progreso

```base
filters:
  and:
    - file.inFolder("45-Freelance-y-Emprendimiento/03 Monetizar hobbies")
    - file.name != "_Índice Monetizar hobbies"
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
