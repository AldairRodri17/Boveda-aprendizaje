---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Modelado 3D técnico
tags: [indice, modelado-3d-tecnico]
contenido: completo
---
# Proyectos técnicos

Parte de: [[37-Modelado-3D-Tecnico/_Índice Modelado 3D técnico|Modelado 3D técnico]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Soportes y organizadores impresos]]
- [[Repuestos y adaptadores impresos]]
- [[Cajas para electrónica]]
- [[Accesorios impresos para foto, video y dron]]

## Laboratorios

- [[Lab 4 - Soporte de iPhone para tripié con tuerca atrapada]]
- [[Lab 5 - Reparar o adaptar una pieza rota (ingeniería inversa)]]

## Progreso

```base
filters:
  and:
    - file.inFolder("37-Modelado-3D-Tecnico/04 Proyectos técnicos")
    - file.name != "_Índice Proyectos técnicos"
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
