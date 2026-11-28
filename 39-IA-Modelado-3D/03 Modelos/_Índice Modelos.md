---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: IA para modelado 3D
tags: [indice, ia-modelado-3d]
contenido: completo
---
# Modelos

Parte de: [[39-IA-Modelado-3D/_Índice IA para modelado 3D|IA para modelado 3D]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[TripoSR]]
- [[Hunyuan3D-2mini]]
- [[Stable Fast 3D]]
- [[TRELLIS]]
- [[Modelos de imagen a 3D en la nube gratuita (Hugging Face y Colab)]]

## Laboratorios

- [[Lab 3 - Hunyuan3D-2mini en poca VRAM vs TripoSR]]
- [[Lab 6 - Modelo pesado en la nube y acabado en local]]

## Progreso

```base
filters:
  and:
    - file.inFolder("39-IA-Modelado-3D/03 Modelos")
    - file.name != "_Índice Modelos"
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
