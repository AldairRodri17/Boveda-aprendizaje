---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Redes
tags: [indice, redes]
contenido: completo
---
# Routing

Parte de: [[19-Redes/_Índice Redes|Redes]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Cómo decide un router (tabla de rutas)]]
- [[Rutas estáticas y ruta por defecto]]
- [[Enrutamiento dinámico (conceptos)]]
  - [[Vector distancia vs estado de enlace]]
  - [[Distancia administrativa y métrica]]
- [[OSPF]]
  - [[OSPF de área única]]
  - [[OSPF multiárea]]
- [[EIGRP]]
- [[BGP (conceptos)]]
- [[Redundancia de gateway (HSRP y VRRP)]]

## Laboratorios

- [[Lab 1 - Rutas estáticas y OSPF de área única en Packet Tracer]]

## Progreso

```base
filters:
  and:
    - file.inFolder("19-Redes/05 Routing")
    - file.name != "_Índice Routing"
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
