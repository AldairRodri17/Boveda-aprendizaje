---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Filosofía práctica
tags: [indice, filosofia]
contenido: completo
---
# Otras escuelas

Parte de: [[66-Filosofia-Practica/_Índice Filosofía práctica|Filosofía práctica]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Epicureísmo]]
- [[Aristóteles y la ética de la virtud]]
- [[Budismo como filosofía práctica]]
- [[Existencialismo (Camus y Sartre)]]
- [[Pensamiento náhuatl y Nezahualcóyotl]] (enlaza con 67)

## Progreso

```base
filters:
  and:
    - file.inFolder("66-Filosofia-Practica/03 Otras escuelas")
    - file.name != "_Índice Otras escuelas"
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
