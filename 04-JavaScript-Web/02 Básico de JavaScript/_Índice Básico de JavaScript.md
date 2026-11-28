---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: JavaScript y Web
tags: [indice, javascript-web]
contenido: completo
---
# Básico de JavaScript

Parte de: [[04-JavaScript-Web/_Índice JavaScript y Web|JavaScript y Web]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Cómo se ejecuta JavaScript (navegador y Node)]]
- [[var, let y const]]
- [[Tipos de datos en JS]]
- [[Coerción de tipos]]
- [[== vs ===]]
- [[Valores truthy y falsy]]
- [[Operadores de null y lógicos en JS]]
- [[Condicionales y bucles en JS]]
- [[Funciones (declaración, expresión y flecha)]]
- [[Alcance (scope)]]
- [[Hoisting]]
- [[Closures]]
- [[this en JavaScript]]
- [[call, apply y bind]]
- [[Objetos]]
- [[Prototipos y herencia prototípica]]
- [[Clases en JS]]
- [[Campos privados en clases JS]]
- [[Destructuring]]
- [[Spread y rest]]
- [[Template literals]]
- [[Arrays y sus métodos]]
- [[map, filter y reduce]]
- [[find, some, every y flatMap]]
- [[sort]]
- [[Map y Set]]
- [[WeakMap y WeakSet]]
- [[JSON.stringify y JSON.parse]]
- [[Fechas en JS]]
- [[Intl (formato de números y fechas)]]
- [[ES Modules e import maps]]
- [[Manejo de errores en JS]]
- [[structuredClone y copia profunda]]

## Laboratorios

- [[Lab 2 - Predecir la salida de JavaScript (coerción, closures y this)]]

## Progreso

```base
filters:
  and:
    - file.inFolder("04-JavaScript-Web/02 Básico de JavaScript")
    - file.name != "_Índice Básico de JavaScript"
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
