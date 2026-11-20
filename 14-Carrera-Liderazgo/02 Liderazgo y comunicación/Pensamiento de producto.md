---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Carrera y liderazgo técnico
tags: [carrera-liderazgo]
---
# Pensamiento de producto

Parte de: [[14-Carrera-Liderazgo/02 Liderazgo y comunicación/_Índice Liderazgo y comunicación|Liderazgo y comunicación]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El **pensamiento de producto** en un desarrollador es entender **el problema del usuario y el objetivo de negocio** detrás de cada tarea, y usar ese entendimiento para decidir qué construir, qué simplificar y cómo medir si funcionó. No te convierte en Product Manager; te convierte en el desarrollador que evita construir lo equivocado.

### Cómo funciona
Antes de escribir código, recorre tres preguntas:

```mermaid
flowchart LR
    Q1[¿Qué problema resuelve<br/>y para quién?] --> Q2[¿Cuál es la solución<br/>más simple que lo resuelve?]
    Q2 --> Q3[¿Cómo sabremos<br/>que funcionó?]
    Q3 --> B[Construir, medir, ajustar]
```

1. **El problema y el usuario.** Las solicitudes suelen llegar como soluciones ("quiero un botón que exporte a Excel"). Detrás hay una necesidad ("necesito saber cada lunes qué sucursales vendieron menos"). Descubrirla cambia lo que construyes.
2. **La solución más simple.** Con la necesidad clara, quizá basta un filtro en pantalla, un correo semanal o un dashboard en Power BI; o confirmas que el Excel es lo correcto y además sabes qué columnas importan.
3. **La métrica, definida antes.** Uso de la funcionalidad, conversión, tiempo ahorrado, tickets de soporte reducidos, errores en el proceso. Si no la defines antes, después cualquier resultado parece éxito.

Cosas que hace un desarrollador con pensamiento de producto:
- Lee los tickets de soporte relacionados con su módulo.
- Pregunta el porqué en el refinamiento.
- Propone cortar alcance cuando algo cuesta mucho y aporta poco.
- Revisa, semanas después, si lo que entregó se usa.

### Ejemplo
**Te piden un reporte exportable a Excel en Wee.Reporteria.**

Preguntas antes de estimar:
- "¿Qué decisión toman con este reporte?"
- "¿Cada cuánto lo necesitan y quién lo usa?"
- "¿Qué hacen hoy para obtener esa información?"
- "¿Cuántas filas esperan? ¿Necesitan fórmulas o solo datos?"

Respuestas posibles: "Lo usa la gerente de operaciones cada lunes para ver qué sucursales están por debajo de la meta; hoy un analista lo arma a mano en 2 horas".

Con eso:
- Quizá la mejor solución es un correo automático cada lunes con las 5 sucursales bajo meta, y el Excel completo como adjunto.
- La métrica: el analista deja de invertir 2 horas semanales; la gerente abre el correo (se puede medir).
- Y sabes qué columnas importan: sucursal, venta, meta, diferencia.

Si después de 4 semanas nadie abre el correo, es información valiosa: el problema real era otro.

### Cuándo usarlo y cuándo no
Úsalo en refinamientos, al estimar, al proponer alcance de MVP ([[Negociar alcance y plazos con negocio]]) y al levantar requisitos ([[Levantamiento de requisitos]]). No lo uses para cuestionar todo: si el PO ya validó la necesidad con datos, no repitas la investigación; aporta desde lo técnico. En tareas internas (un refactor, una migración) el "usuario" es el equipo: la métrica puede ser tiempo de compilación o incidentes.

### Errores comunes
1. **Construir literalmente lo pedido** sin preguntar el problema, y descubrir después que no se usa.
2. **Cuestionar por cuestionar.** Preguntar el porqué es para entender, no para bloquear. Hazlo con curiosidad, no como interrogatorio.
3. **No definir la métrica antes.** Sin ella no hay forma honesta de saber si sirvió.
4. **Enamorarte de la solución técnica.** Elegir la arquitectura más interesante en lugar de la más útil.
5. **No volver a revisar.** Entregar y olvidar; el aprendizaje está en ver qué pasó después.

### Para profundizar
- [RICE: Simple prioritization for product managers — Intercom](https://www.intercom.com/blog/rice-simple-prioritization-for-product-managers/)
- [User stories — Agile Alliance](https://agilealliance.org/agile101/user-stories)
- [INVEST — Agile Alliance](https://agilealliance.org/glossary/invest)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es pensamiento de producto en un desarrollador?
> Entender el problema del usuario y el objetivo de negocio detrás de cada tarea, y usarlo para decidir qué construir, qué simplificar y cómo medir si funcionó.

> [!question]- Reflexión: te piden un reporte en Excel exportable. ¿Qué preguntas? Respuesta modelo.
> "¿Qué decisión toman con ese reporte? ¿Cada cuánto? ¿Quién lo usa?" Quizá basta con un filtro en pantalla o un correo semanal; o confirmas que el Excel es lo correcto y sabes qué columnas importan.

> [!question]- ¿Cómo mides si una funcionalidad sirvió?
> Con una métrica definida antes: uso, conversión, tiempo ahorrado, tickets de soporte reducidos.

