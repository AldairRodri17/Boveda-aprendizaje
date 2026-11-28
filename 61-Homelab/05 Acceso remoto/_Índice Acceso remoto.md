---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Homelab y self-hosting
tags: [indice, homelab]
contenido: completo
---
# Acceso remoto

Parte de: [[61-Homelab/_Índice Homelab y self-hosting|Homelab y self-hosting]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[VPN con WireGuard]] ⭐
- [[Tailscale]]
- [[Cloudflare Tunnel]]
- [[Riesgos de exponer servicios a internet]] ⭐

## Laboratorios

- [[Lab 3 - Acceso remoto seguro con WireGuard o Tailscale]]

## Progreso

```base
filters:
  and:
    - file.inFolder("61-Homelab/05 Acceso remoto")
    - file.name != "_Índice Acceso remoto"
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
