---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Integraciones
tags: [indice, integraciones]
---
# Correo

Parte de: [[22-Integraciones/_Índice Integraciones|Integraciones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Correo transaccional (SMTP y proveedores)]]
- [[SPF, DKIM y DMARC]]
  - [[SPF (Sender Policy Framework)]]
  - [[DKIM (firma de correo)]]
  - [[DMARC (política y reportes)]]

## Laboratorios

- [[Lab 2 - Verificar SPF, DKIM y DMARC de tu dominio]]

## Progreso

```base
filters:
  and:
    - file.inFolder("22-Integraciones/02 Correo")
    - file.name != "_Índice Correo"
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
