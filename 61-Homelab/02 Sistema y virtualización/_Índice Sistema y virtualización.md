---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Homelab y self-hosting
tags: [indice, homelab]
contenido: completo
---
# Sistema y virtualización

Parte de: [[61-Homelab/_Índice Homelab y self-hosting|Homelab y self-hosting]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Proxmox]] ⭐
- [[Máquinas virtuales vs contenedores]]
- [[Linux para desarrolladores (SSH, permisos, procesos y systemd)]] (vive en 02 · Fundamentos)
- [[Docker]] (vive en 08)
- [[Portainer y Dockge]]

## Laboratorios

- [[Lab 1 - Proxmox en una PC vieja]]

## Progreso

```base
filters:
  and:
    - file.inFolder("61-Homelab/02 Sistema y virtualización")
    - file.name != "_Índice Sistema y virtualización"
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
