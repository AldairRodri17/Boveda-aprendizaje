---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Equipo
tags: [indice, equipo]
---
# Compras y accesorios

Parte de: [[34-Equipo/_Índice Equipo|Equipo]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Qué comprar primero (guía para principiantes)]]
- [[Tarjetas de memoria (clases de velocidad)]]
- [[SSD externo para grabar y respaldar]]
- [[Tripiés y soportes]]
- [[Filtros ND y Black Mist para video]]
- [[Luces LED portátiles]]
- [[Computadora para editar foto y video]]
- [[Cuidado del equipo en viaje, calle y lluvia]]

## Laboratorios

- [[Lab 4 - Plan de almacenamiento y respaldo de mis grabaciones]]

## Progreso

```base
filters:
  and:
    - file.inFolder("34-Equipo/03 Compras y accesorios")
    - file.name != "_Índice Compras y accesorios"
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
