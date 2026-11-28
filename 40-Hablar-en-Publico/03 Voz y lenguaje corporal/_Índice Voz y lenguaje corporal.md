---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Hablar en público y exposiciones
tags: [indice, hablar-en-publico]
---
# Voz y lenguaje corporal

Parte de: [[40-Hablar-en-Publico/_Índice Hablar en público y exposiciones|Hablar en público y exposiciones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Ritmo, pausas y volumen]] ⭐
- [[Muletillas y cómo reducirlas]] ⭐
- [[Postura, gestos y manos]]
- [[Mirada y conexión con el público]]
- [[Uso del espacio en el escenario]]

## Progreso

```base
filters:
  and:
    - file.inFolder("40-Hablar-en-Publico/03 Voz y lenguaje corporal")
    - file.name != "_Índice Voz y lenguaje corporal"
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
