---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Radar de herramientas
herramienta: Obsidian y Mermaid
tags: [radar-herramientas, laboratorio]
contenido: completo
---
# Lab 1 - Armar mi radar personal de herramientas

Parte de: [[10-Radar-Herramientas/_Índice Radar de herramientas|Radar de herramientas]]

**Cubre:** [[Radar - cómo usarlo]] · [[Radar - IA para desarrollo]] · [[Mermaid y diagramas como código]] · [[Obsidian (frontmatter, Dataview y plugins útiles)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Construir tu propio radar en esta bóveda: una nota por herramienta con propiedades, una vista que las agrupe por anillo y un diagrama Mermaid del radar.

## Pasos
1. Elige 8–12 herramientas que usas o te interesan (al menos una de IA, una de observabilidad, una de testing y una de bases de datos).
2. En cada nota de herramienta de esta área (o en notas nuevas en una subcarpeta `Mi radar`) agrega las propiedades `anillo` (adoptar, probar, evaluar, detener), `cuadrante` (técnicas, herramientas, plataformas, lenguajes) y `revisado` (fecha).
3. Escribe en cada una 3 líneas: qué problema resuelve, por qué está en ese anillo, qué te haría moverla.
4. Crea una nota `Mi radar` con un bloque `base` que muestre las notas con `anillo`, agrupadas o filtradas por anillo.
5. Dibuja el radar en Mermaid (por ejemplo un `mindmap` o un `flowchart` con un subgrafo por anillo).
6. Agenda en tu diario una revisión dentro de 3 meses.

## Criterios de terminado
- [ ] Hay al menos 8 herramientas con `anillo`, `cuadrante` y `revisado` llenos con valores consistentes.
- [ ] La vista de `Mi radar` muestra todas y permite ver cuáles están en *adoptar*.
- [ ] Cada herramienta tiene escrito el porqué de su anillo y qué la movería.
- [ ] El diagrama Mermaid renderiza sin errores.
- [ ] Hay una fecha de revisión agendada.

## Solución

> [!success]- Vista de Bases de ejemplo
> ```base
> filters:
>   and:
>     - anillo != null
> views:
>   - type: table
>     name: Adoptar
>     filters:
>       and:
>         - anillo == "adoptar"
>     order:
>       - file.name
>       - cuadrante
>       - revisado
>   - type: table
>     name: Todo el radar
>     order:
>       - anillo
>       - file.name
>       - cuadrante
> ```

> [!success]- Radar en Mermaid
> ```mermaid
> flowchart TB
>   subgraph Adoptar
>     A1[OpenTelemetry] --- A2[Archivos .http] --- A3[Testcontainers]
>   end
>   subgraph Probar
>     P1[.NET Aspire] --- P2[Bruno]
>   end
>   subgraph Evaluar
>     E1[Asistente de IA en revisión de PR]
>   end
>   subgraph Detener
>     D1[Colecciones con secretos en la nube]
>   end
> ```

## Notas personales

_Qué me costó, errores que cometí, dudas._
