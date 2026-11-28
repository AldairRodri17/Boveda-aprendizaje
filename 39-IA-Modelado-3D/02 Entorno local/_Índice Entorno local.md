---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: IA para modelado 3D
tags: [indice, ia-modelado-3d]
contenido: completo
---
# Entorno local

Parte de: [[39-IA-Modelado-3D/_Índice IA para modelado 3D|IA para modelado 3D]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Python, CUDA y PyTorch para IA 3D en Windows]]
- [[VRAM - medirla y qué hacer cuando no alcanza]]
- [[Tarjetas GTX 16 y media precisión (FP16)]]
- [[ComfyUI para generación 3D]]
- [[Interfaces Gradio de los modelos 3D]]

## Laboratorios

- [[Lab 1 - Preparar Python, CUDA y PyTorch y verificar la GPU]]
- [[Lab 2 - Primer modelo con TripoSR desde una foto propia]]

## Progreso

```base
filters:
  and:
    - file.inFolder("39-IA-Modelado-3D/02 Entorno local")
    - file.name != "_Índice Entorno local"
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
