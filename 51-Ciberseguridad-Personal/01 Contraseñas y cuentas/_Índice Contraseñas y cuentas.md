---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Ciberseguridad personal y vida digital
tags: [indice, ciberseguridad-personal]
---
# Contraseñas y cuentas

Parte de: [[51-Ciberseguridad-Personal/_Índice Ciberseguridad personal y vida digital|Ciberseguridad personal y vida digital]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Gestor de contraseñas]] ⭐
- [[Contraseñas fuertes y frases de paso]]
- [[Verificación en dos pasos (2FA) y llaves de seguridad]] ⭐
- [[Passkeys]]
- [[Recuperación de cuentas]]

## Laboratorios

- [[Lab 1 - Auditoría de mis cuentas y activar 2FA]]

## Progreso

```base
filters:
  and:
    - file.inFolder("51-Ciberseguridad-Personal/01 Contraseñas y cuentas")
    - file.name != "_Índice Contraseñas y cuentas"
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
