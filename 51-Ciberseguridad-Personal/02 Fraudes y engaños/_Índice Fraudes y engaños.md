---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Ciberseguridad personal y vida digital
tags: [indice, ciberseguridad-personal]
contenido: completo
---
# Fraudes y engaños

Parte de: [[51-Ciberseguridad-Personal/_Índice Ciberseguridad personal y vida digital|Ciberseguridad personal y vida digital]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Phishing, smishing y vishing]] ⭐
- [[Fraudes por WhatsApp y llamadas en México]] ⭐
- [[Compras en línea seguras]]
- [[Fraudes con IA (voces y videos falsos)]]

## Progreso

```base
filters:
  and:
    - file.inFolder("51-Ciberseguridad-Personal/02 Fraudes y engaños")
    - file.name != "_Índice Fraudes y engaños"
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
