---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: IA y Machine Learning
tags: [indice, ia-machine-learning]
---
# Agentes de IA

Parte de: [[21-IA-Machine-Learning/_Índice IA y Machine Learning|IA y Machine Learning]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Qué es un agente de IA]]
- [[El ciclo de un agente (razonar, actuar, observar)]]
- [[Herramientas y MCP (Model Context Protocol)]]
- [[Memoria de agentes (corto y largo plazo)]]
- [[Planificación y descomposición de tareas]]
- [[Sistemas multi-agente y orquestación]]
- [[Humano en el ciclo (aprobaciones)]]
- [[Guardrails y límites de un agente]]
- [[Evaluación de agentes]]
- [[Frameworks de agentes (comparativa)]]
- [[Agentes de código (asistentes de programación)]]
- [[Computer use y agentes de navegador]]

## Laboratorios

- [[Lab 6 - Agente con tool calling y un servidor MCP sencillo]]
- [[Lab 7 - Evaluación de respuestas de un LLM]]

## Progreso

```base
filters:
  and:
    - file.inFolder("21-IA-Machine-Learning/06 Agentes de IA")
    - file.name != "_Índice Agentes de IA"
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
