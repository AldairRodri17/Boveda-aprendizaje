---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: IA para modelado 3D
tags: [indice, ia-modelado-3d]
contenido: completo
---
# Licencias y ética

Parte de: [[39-IA-Modelado-3D/_Índice IA para modelado 3D|IA para modelado 3D]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Licencias de modelos de IA 3D]]
  - [[Licencia MIT en modelos de IA 3D]]
  - [[Licencia Stability AI Community]]
  - [[Licencia Tencent Hunyuan 3D Community]]
- [[Propiedad y uso comercial de modelos generados por IA]]

## Progreso

```base
filters:
  and:
    - file.inFolder("39-IA-Modelado-3D/06 Licencias y ética")
    - file.name != "_Índice Licencias y ética"
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
