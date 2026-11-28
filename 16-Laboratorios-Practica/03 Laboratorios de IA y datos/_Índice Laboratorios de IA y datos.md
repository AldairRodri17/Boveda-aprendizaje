---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Laboratorios y práctica
tags: [indice, laboratorios-practica]
contenido: completo
---
# Laboratorios de IA y datos

Parte de: [[16-Laboratorios-Practica/_Índice Laboratorios y práctica|Laboratorios y práctica]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Google Colab]]
- [[Kaggle]]
- [[Hugging Face]]
- [[Modelos locales con Ollama]]

## Progreso

```base
filters:
  and:
    - file.inFolder("16-Laboratorios-Practica/03 Laboratorios de IA y datos")
    - file.name != "_Índice Laboratorios de IA y datos"
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
