---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Redes
tags: [indice, redes]
---
# Direccionamiento IP

Parte de: [[19-Redes/_Índice Redes|Redes]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[IPv4 - estructura y clases]]
- [[Direcciones públicas y privadas]]
  - [[IP pública]]
  - [[IP privada (RFC 1918)]]
  - [[Loopback y APIPA]]
  - [[Dirección de red y de broadcast]]
- [[Máscara de subred y CIDR]]
- [[Subnetting paso a paso]]
- [[VLSM]]
- [[IPv6 - direccionamiento]]
  - [[Formato y abreviación de IPv6]]
  - [[IPv6 global unicast]]
  - [[IPv6 link-local]]
  - [[IPv6 multicast]]
- [[IPv6 - SLAAC y DHCPv6]]
- [[ARP]]
- [[ICMP, ping y traceroute]]

## Laboratorios

- [[Lab 1 - Subredes IPv4 en Packet Tracer]]
- [[Lab 2 - IPv6 con SLAAC en Packet Tracer]]

## Progreso

```base
filters:
  and:
    - file.inFolder("19-Redes/03 Direccionamiento IP")
    - file.name != "_Índice Direccionamiento IP"
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
