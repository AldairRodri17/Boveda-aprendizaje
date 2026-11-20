---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Homelab y self-hosting
tags: [indice, homelab]
---
# Red del homelab

Parte de: [[61-Homelab/_Índice Homelab y self-hosting|Homelab y self-hosting]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[IP fija y reservas DHCP en casa]]
- [[VLANs en casa]]
- [[DNS local y bloqueo de anuncios (Pi-hole y AdGuard Home)]]
- [[Proxy inverso en el homelab (Nginx Proxy Manager, Caddy y Traefik)]] ⭐
- [[Certificados HTTPS en el homelab]]

## Laboratorios

- [[Lab 4 - Proxy inverso con HTTPS]]

## Progreso

```base
filters:
  and:
    - file.inFolder("61-Homelab/04 Red del homelab")
    - file.name != "_Índice Red del homelab"
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
