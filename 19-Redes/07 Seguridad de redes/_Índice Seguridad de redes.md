---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Redes
tags: [indice, redes]
---
# Seguridad de redes

Parte de: [[19-Redes/_Índice Redes|Redes]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[ACLs (estándar y extendidas)]]
  - [[ACL estándar]]
  - [[ACL extendida]]
  - [[ACL nombrada]]
- [[Firewalls (stateless, stateful y de nueva generación)]]
- [[VPN (sitio a sitio y acceso remoto, IPsec y SSL)]]
  - [[VPN sitio a sitio]]
  - [[VPN de acceso remoto]]
  - [[IPsec]]
  - [[VPN SSL]]
- [[Segmentación y DMZ]]
- [[Zero Trust en redes]]
- [[Ataques comunes de red (spoofing, MITM, DoS)]]

## Laboratorios

- [[Lab 1 - ACLs estándar y extendidas en Packet Tracer]]

## Progreso

```base
filters:
  and:
    - file.inFolder("19-Redes/07 Seguridad de redes")
    - file.name != "_Índice Seguridad de redes"
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
