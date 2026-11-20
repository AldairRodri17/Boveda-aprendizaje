---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Escritura clara
tags: [indice, escritura]
---
# Redacción clara

Parte de: [[50-Escritura/_Índice Escritura clara|Escritura clara]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Escribir claro (frases cortas y una idea por párrafo)]] ⭐
- [[Pirámide invertida (lo importante primero)]] ⭐
- [[Quitar palabras de relleno]]
- [[Revisar y corregir tus propios textos]] ⭐
- [[Escribir para leer en pantalla]]

## Progreso

```base
filters:
  and:
    - file.inFolder("50-Escritura/05 Redacción clara")
    - file.name != "_Índice Redacción clara"
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
