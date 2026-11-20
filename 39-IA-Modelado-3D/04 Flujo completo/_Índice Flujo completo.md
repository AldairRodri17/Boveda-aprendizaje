---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: IA para modelado 3D
tags: [indice, ia-modelado-3d]
---
# Flujo completo

Parte de: [[39-IA-Modelado-3D/_Índice IA para modelado 3D|IA para modelado 3D]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Crear la imagen de entrada para IA 3D]]
- [[Stable Diffusion en 6 GB de VRAM]]
- [[Quitar el fondo de una imagen]]
- [[Limpiar un modelo generado por IA en Blender]]
- [[Separar un modelo por colores para CANVAS]]
- [[De la IA a la impresora (flujo completo)]]

## Laboratorios

- [[Lab 4 - Del dibujo al objeto impreso con IA 3D]]
- [[Lab 5 - Separar un modelo generado en 4 colores para CANVAS]]

## Progreso

```base
filters:
  and:
    - file.inFolder("39-IA-Modelado-3D/04 Flujo completo")
    - file.name != "_Índice Flujo completo"
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
