---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Redes
tags: [indice, redes]
contenido: completo
---
# Switching

Parte de: [[19-Redes/_Índice Redes|Redes]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Cómo aprende un switch (tabla MAC)]]
- [[Unicast, broadcast y multicast]]
- [[VLANs]]
  - [[VLAN nativa]]
  - [[VLAN de administración]]
  - [[VLAN de voz]]
- [[Trunking 802.1Q]]
- [[Routing entre VLANs (router on a stick y switch capa 3)]]
  - [[Router on a stick]]
  - [[Switch capa 3 (SVI)]]
- [[STP (Spanning Tree) y RSTP]]
- [[EtherChannel (LACP y PAgP)]]
- [[Port security]]
- [[DHCP snooping y Dynamic ARP Inspection]]

## Laboratorios

- [[Lab 1 - VLANs, trunk y router on a stick en Packet Tracer]]
- [[Lab 2 - STP y EtherChannel en Packet Tracer]]

## Progreso

```base
filters:
  and:
    - file.inFolder("19-Redes/02 Switching")
    - file.name != "_Índice Switching"
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
