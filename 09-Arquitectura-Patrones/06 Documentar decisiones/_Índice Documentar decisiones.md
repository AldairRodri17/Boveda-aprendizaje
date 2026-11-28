---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Arquitectura y patrones
tags: [indice, arquitectura-patrones]
contenido: completo
---
# Documentar decisiones

Parte de: [[09-Arquitectura-Patrones/_Índice Arquitectura y patrones|Arquitectura y patrones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[ADR - registro de decisiones de arquitectura]]
- [[Design docs y RFCs]]
- [[Diagramas C4]]
  - [[C4 nivel 1 - Contexto]]
  - [[C4 nivel 2 - Contenedores]]
  - [[C4 nivel 3 - Componentes]]
  - [[C4 nivel 4 - Código]]
- [[Diagramas de secuencia]]
- [[Gestión de deuda técnica]]
- [[Modernizar código legado]]
- [[Cómo elegir una arquitectura]]

## Laboratorios

- [[Lab 5 - Escribir un ADR real en ARQUITECTURA.md]]

## Progreso

```base
filters:
  and:
    - file.inFolder("09-Arquitectura-Patrones/06 Documentar decisiones")
    - file.name != "_Índice Documentar decisiones"
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
