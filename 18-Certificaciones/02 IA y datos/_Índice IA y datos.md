---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Certificaciones
tags: [indice, certificaciones]
contenido: completo
---
# IA y datos

Parte de: [[18-Certificaciones/_Índice Certificaciones|Certificaciones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[AI-900 Azure AI Fundamentals]]
- [[AI-102 Azure AI Engineer Associate]]
- [[Certificaciones de machine learning (Azure, AWS y Google)]]
- [[TensorFlow y PyTorch - certificados y cursos reconocidos]]

## Progreso

```base
filters:
  and:
    - file.inFolder("18-Certificaciones/02 IA y datos")
    - file.name != "_Índice IA y datos"
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
