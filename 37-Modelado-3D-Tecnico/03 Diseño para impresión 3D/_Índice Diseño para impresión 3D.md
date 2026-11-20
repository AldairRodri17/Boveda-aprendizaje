---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Modelado 3D técnico
tags: [indice, modelado-3d-tecnico]
---
# Diseño para impresión 3D

Parte de: [[37-Modelado-3D-Tecnico/_Índice Modelado 3D técnico|Modelado 3D técnico]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Orientación de la pieza y resistencia por capas]]
- [[Voladizos y puentes]]
- [[Tolerancias y holguras para piezas que encajan]]
  - [[Ajuste a presión en piezas impresas]]
  - [[Ajuste deslizante y giratorio en piezas impresas]]
  - [[Holgura para piezas impresas en su lugar (print-in-place)]]
- [[Agujeros, roscas impresas, insertos y tuercas atrapadas]]
- [[Clips, bisagras impresas y piezas en varias partes]]
- [[Grosor de pared y relleno según la carga]]
- [[Medir piezas e ingeniería inversa]]

## Laboratorios

- [[Lab 3 - Bisagra impresa en su lugar]]

## Progreso

```base
filters:
  and:
    - file.inFolder("37-Modelado-3D-Tecnico/03 Diseño para impresión 3D")
    - file.name != "_Índice Diseño para impresión 3D"
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
