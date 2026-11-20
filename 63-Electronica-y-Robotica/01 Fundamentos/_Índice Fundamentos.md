---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Electrónica y robótica
tags: [indice, electronica]
---
# Fundamentos

Parte de: [[63-Electronica-y-Robotica/_Índice Electrónica y robótica|Electrónica y robótica]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Voltaje, corriente y resistencia (ley de Ohm)]] ⭐
- [[Circuitos en serie y en paralelo]]
- [[Componentes básicos (resistencias, capacitores, diodos y transistores)]] ⭐
- [[Leer diagramas electrónicos]]
- [[Seguridad eléctrica]] ⭐

## Progreso

```base
filters:
  and:
    - file.inFolder("63-Electronica-y-Robotica/01 Fundamentos")
    - file.name != "_Índice Fundamentos"
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
