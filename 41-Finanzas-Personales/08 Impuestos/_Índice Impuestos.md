---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Salud financiera
tags: [indice, finanzas]
---
# Impuestos

Parte de: [[41-Finanzas-Personales/_Índice Salud financiera|Salud financiera]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Impuestos para asalariados (ISR y retenciones)]]
- [[Declaración anual de personas físicas]] ⭐
- [[Deducciones personales]]
- [[RFC, e.firma y Buzón Tributario]]
- [[Facturación (CFDI) para personas físicas]]
- [[Impuestos como freelance]] (enlaza con 45)

## Laboratorios

- [[Lab 5 - Mi declaración anual con deducciones]]

## Progreso

```base
filters:
  and:
    - file.inFolder("41-Finanzas-Personales/08 Impuestos")
    - file.name != "_Índice Impuestos"
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
