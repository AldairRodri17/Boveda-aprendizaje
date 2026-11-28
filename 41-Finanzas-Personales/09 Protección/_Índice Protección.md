---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Salud financiera
tags: [indice, finanzas]
contenido: completo
---
# Protección

Parte de: [[41-Finanzas-Personales/_Índice Salud financiera|Salud financiera]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Fraudes financieros comunes en México]] ⭐
- [[Esquemas piramidales e inversiones milagro]] ⭐
- [[Seguridad en banca digital]] (enlaza con 51)
- [[CONDUSEF - cómo presentar una queja]]

## Progreso

```base
filters:
  and:
    - file.inFolder("41-Finanzas-Personales/09 Protección")
    - file.name != "_Índice Protección"
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
