---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Aprender a aprender y productividad
tags: [indice, productividad]
contenido: completo
---
# Aprender a aprender

Parte de: [[43-Aprender-y-Productividad/_Índice Aprender a aprender y productividad|Aprender a aprender y productividad]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Cómo funciona la memoria]]
- [[Recuperación activa]] ⭐
- [[Repaso espaciado]] ⭐
- [[Técnica Feynman]] ⭐
- [[Práctica deliberada en cualquier habilidad]]
- [[Intercalar temas (interleaving)]]
- [[Aprender haciendo proyectos]]

## Laboratorios

- [[Lab 1 - Explicar un tema técnico con la técnica Feynman]]

## Progreso

```base
filters:
  and:
    - file.inFolder("43-Aprender-y-Productividad/01 Aprender a aprender")
    - file.name != "_Índice Aprender a aprender"
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
