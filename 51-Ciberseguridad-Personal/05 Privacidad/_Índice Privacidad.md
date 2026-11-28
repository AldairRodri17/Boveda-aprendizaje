---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Ciberseguridad personal y vida digital
tags: [indice, ciberseguridad-personal]
contenido: completo
---
# Privacidad

Parte de: [[51-Ciberseguridad-Personal/_Índice Ciberseguridad personal y vida digital|Ciberseguridad personal y vida digital]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Privacidad en redes sociales]]
- [[Huella digital y datos personales]]
- [[Permisos de las apps]]
- [[Derechos ARCO sobre tus datos personales]]

## Progreso

```base
filters:
  and:
    - file.inFolder("51-Ciberseguridad-Personal/05 Privacidad")
    - file.name != "_Índice Privacidad"
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
