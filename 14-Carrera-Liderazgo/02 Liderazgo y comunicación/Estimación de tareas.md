---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Carrera y liderazgo técnico
tags: [carrera-liderazgo]
---
# Estimación de tareas

Parte de: [[14-Carrera-Liderazgo/02 Liderazgo y comunicación/_Índice Liderazgo y comunicación|Liderazgo y comunicación]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Estimar** es predecir cuánto esfuerzo o tiempo tomará un trabajo, con la incertidumbre que tiene. Una buena estimación no es un número exacto, sino un **rango honesto** que permite planear y negociar. Para un senior es clave: de ella dependen las fechas que el negocio promete.

### Cómo funciona
**¿Por qué rangos?** La incertidumbre es real: dependencias, código desconocido, requisitos que cambian. Un número único ("5 días") se escucha como compromiso; un rango ("4 a 7 días, más probable 5") comunica el riesgo.

Técnicas que ayudan:
1. **Dividir** en tareas de 1–2 días como máximo. Lo grande se estima mal.
2. **Comparar con trabajo pasado.** "El endpoint de reembolsos se parece al de cancelaciones, que tomó 3 días."
3. **Puntos de historia** relativos, con planning poker para que todo el equipo opine sin anclarse en el primero que habla.
4. **Estimación de tres puntos (PERT):** optimista (O), más probable (M) y pesimista (P).
   - Estimado: `E = (O + 4M + P) / 6`
   - Desviación estándar: `σ = (P − O) / 6`
5. **Incluir todo:** pruebas, revisión, migraciones, despliegue, documentación y comunicación.

```mermaid
flowchart LR
    T[Historia] --> D[Dividir en tareas ≤ 2 días]
    D --> C[Comparar con histórico]
    C --> R[Rango O / M / P]
    R --> A[Sumar pruebas, review, deploy]
    A --> COM[Comunicar rango y supuestos]
```

**Con velocidad del equipo:** si el equipo termina en promedio 30 puntos por sprint de 2 semanas y el backlog restante suma 135 puntos, se necesitan 135 / 30 = 4.5 sprints; como no hay medios sprints, son **5 sprints, unas 10 semanas**. Y conviene decirlo como rango, porque la velocidad varía.

### Ejemplo
**PERT para la integración con una nueva pasarela en Itzamna.Dionisio:** optimista 2 días, más probable 4, pesimista 12.

```text
E = (2 + 4×4 + 12) / 6 = 30 / 6 = 5 días
σ = (12 − 2) / 6 = 10 / 6 ≈ 1.67 días
```

Fíjate que el estimado (5) es mayor que el más probable (4): el pesimista tan alto jala el resultado. Eso indica un riesgo concreto que conviene nombrar.

Cómo comunicarlo:
> "Estimo 5 días, en un rango de 4 a 7. El riesgo principal es que el sandbox del proveedor no tenga documentados los webhooks; si es así, puede irse a 12. Mañana lo confirmo con una prueba de 2 horas y ajusto la estimación."

Desglose que respalda el número:

| Tarea | Días |
|---|---|
| Adaptador HTTP y configuración (secretos en Key Vault) | 1 |
| Procedimiento `usp_Pago_Registrar` + archivo `.sql` | 0.5 |
| Webhook con [[Idempotencia]] | 1 |
| Pruebas de integración contra sandbox | 1 |
| Review, ajustes y despliegue a QA | 1 |
| Margen por documentación incierta | 0.5 |

### Cuándo usarlo y cuándo no
Usa PERT o rangos cuando la fecha importa o el trabajo es nuevo. Para trabajo muy repetitivo, el histórico basta. En equipos con Kanban, en lugar de estimar cada tarjeta se usa el *throughput* y el *cycle time* para pronosticar ([[Kanban]]). Si la incertidumbre es enorme (tecnología nueva), haz primero un *spike* acotado en tiempo y estima después. Para negociar con esa estimación, ver [[Negociar alcance y plazos con negocio]].

### Errores comunes
1. **Estimar solo el camino feliz del código** y olvidar pruebas, revisión, migraciones y despliegue.
2. **Dar un número único** que luego se toma como promesa.
3. **Ceder a la presión** y bajar la estimación porque "se ve mucho". Cambia el alcance, no el número.
4. **No re-estimar** cuando aparece información nueva. Avisa en cuanto lo sepas.
5. **Comparar puntos entre equipos.** Los puntos son relativos a cada equipo; no son productividad.

### Para profundizar
- [Planning Poker — Mountain Goat Software](https://www.mountaingoatsoftware.com/agile/planning-poker)
- [The 2020 Scrum Guide](https://scrumguides.org/scrum-guide.html)
- [The Kanban Guide](https://kanbanguides.org/english/)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- PERT: optimista 2 días, más probable 4, pesimista 12. Calcula el estimado y la desviación estándar.
> E = (O + 4M + P) / 6 = (2 + 16 + 12) / 6 = **5 días**. σ = (P − O) / 6 = 10/6 ≈ **1.7 días**. Comunica "≈ 5 días, probablemente entre 3.3 y 6.7".

> [!question]- Tu velocidad promedio es 30 puntos por sprint de 2 semanas. El backlog restante suma 135 puntos. ¿Cuántos sprints?
> 135 / 30 = 4.5 → **5 sprints (10 semanas)**. Avisa que es una proyección, no una promesa.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Por qué estimar en rangos y no en un número?
> La incertidumbre es real; un rango ("3–5 días") comunica riesgo. Un número único se toma como compromiso.

> [!question]- ¿Qué técnicas ayudan?
> Dividir en tareas de ≤ 1–2 días, comparar con trabajo pasado, puntos de historia relativos, estimación de tres puntos (PERT), y agregar tiempo para pruebas, revisión y despliegue.

> [!question]- Error común.
> Estimar solo el código del camino feliz y olvidar pruebas, revisión, migraciones, despliegue y comunicación.

