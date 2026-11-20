---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Homelab y self-hosting
tags: [indice, homelab]
---
# Servicios

Parte de: [[61-Homelab/_Índice Homelab y self-hosting|Homelab y self-hosting]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Nextcloud (nube propia)]]
- [[Immich (fotos propias)]] ⭐
- [[Respaldar fotos y videos en el homelab]]
- [[Jellyfin (servidor de medios)]]
- [[Vaultwarden (gestor de contraseñas propio)]]
- [[Monitoreo del homelab (Uptime Kuma y Grafana)]]
- [[Gitea (Git propio)]]
- [[Modelos locales con Ollama]] (vive en 16)

## Laboratorios

- [[Lab 2 - Immich con respaldo de fotos del iPhone]]

## Progreso

```base
filters:
  and:
    - file.inFolder("61-Homelab/06 Servicios")
    - file.name != "_Índice Servicios"
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
