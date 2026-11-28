---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Laboratorios y práctica
tags: [indice, laboratorios-practica]
contenido: completo
---
# Plataformas de despliegue con plan gratuito

Parte de: [[16-Laboratorios-Practica/_Índice Laboratorios y práctica|Laboratorios y práctica]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Cloudflare (Pages y Workers)]]
- [[Cloudflare R2]]
- [[Vercel]]
- [[Netlify]]
- [[Render]]
- [[Neon (PostgreSQL)]]
- [[Supabase]]
- [[Firebase]]

## Progreso

```base
filters:
  and:
    - file.inFolder("16-Laboratorios-Practica/06 Plataformas de despliegue con plan gratuito")
    - file.name != "_Índice Plataformas de despliegue con plan gratuito"
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
