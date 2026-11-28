---
proyecto: Conocimiento
tipo: laboratorio
nivel: avanzado
progreso: pendiente
estado: borrador
area: Carrera y liderazgo técnico
herramienta: Excalidraw o papel, Mermaid, cronómetro
tags: [carrera-liderazgo, laboratorio]
contenido: completo
---
# Lab 4 - Simulacro de entrevista de diseño de sistemas

Parte de: [[14-Carrera-Liderazgo/04 Búsqueda de trabajo/_Índice Búsqueda de trabajo|Búsqueda de trabajo]]

**Cubre:** [[Entrevistas de diseño de sistemas]] · [[Preparación para entrevistas técnicas]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Hacer un simulacro realista de 45 minutos de diseño de sistemas, pensando en voz alta, y evaluarte con una rúbrica.

## Pasos
1. Elige un caso que **no** hayas resuelto aún de [[Diseño de sistemas - cómo abordarlo|la sección de diseño de sistemas]] (p. ej. [[Caso - sistema de notificaciones]] o [[Caso - chat en tiempo real]]).
2. Pide a alguien que haga de entrevistador (o grábate en video). Si estás solo, lee el enunciado y no veas la solución.
3. Sigue los tiempos: 5 min requisitos (haz preguntas), 5 estimaciones, 10 alto nivel con diagrama, 15 profundizar en 2 componentes, 10 cuellos de botella, fallos y trade-offs.
4. Al terminar, califícate con la rúbrica de la solución y compara con la solución guiada del caso.
5. Repite una semana después con otro caso y compara calificaciones.

## Criterios de terminado
- [ ] Hiciste al menos 4 preguntas de requisitos antes de diseñar.
- [ ] Las estimaciones tienen números (req/s, almacenamiento) y cambiaron alguna decisión.
- [ ] El diagrama tiene todos los componentes con flechas etiquetadas.
- [ ] Mencionaste al menos 3 trade-offs explícitos y qué pasa si cae un componente.
- [ ] Terminaste en 45 min y te calificaste con la rúbrica (≥ 12/16 en el segundo intento).

## Solución

> [!success]- Rúbrica (0–2 por fila, máx. 16)
> | Criterio | 0 | 1 | 2 |
> |---|---|---|---|
> | Requisitos | No preguntó | Algunas preguntas | Funcionales, no funcionales y alcance claros |
> | Estimaciones | Ninguna | Números sin uso | Números que guían decisiones |
> | API y datos | Ausente | Parcial | Endpoints y modelo claros |
> | Alto nivel | Confuso | Correcto pero incompleto | Completo y justificado |
> | Profundidad | Superficial | Un componente | Dos componentes con detalle |
> | Fallos | No los trata | Menciona | Plan concreto (reintentos, DLQ, failover) |
> | Trade-offs | Ninguno | Implícitos | Explícitos y ligados a requisitos |
> | Comunicación | Silencios largos | A ratos | Piensa en voz alta y estructura |

> [!success]- Frases útiles en inglés
> "Let me start by clarifying the requirements." · "Let's do a quick back-of-the-envelope estimate." · "The main trade-off here is…" · "If this component fails, we…"

## Notas personales

_Qué me costó, errores que cometí, dudas._
