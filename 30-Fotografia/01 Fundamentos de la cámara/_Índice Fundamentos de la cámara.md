---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Fotografía
tags: [indice, fotografia]
contenido: completo
---
# Fundamentos de la cámara

Parte de: [[30-Fotografia/_Índice Fotografía|Fotografía]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Tipos de cámara (celular, compacta, mirrorless y réflex)]]
- [[Tamaño de sensor y factor de recorte]]
- [[Distancia focal y tipos de lente]]
- [[Modos de disparo (Auto, P, A, S y M)]]
- [[RAW vs JPEG y HEIF]]
- [[Tarjetas, baterías y cuidado del equipo fotográfico]]

## Laboratorios

- [[Práctica Foto 01 - Las tres cámaras del iPhone y los formatos]]

## Progreso

```base
filters:
  and:
    - file.inFolder("30-Fotografia/01 Fundamentos de la cámara")
    - file.name != "_Índice Fundamentos de la cámara"
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
