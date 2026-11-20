---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
---
# Narrativa con datos (storytelling)

Parte de: [[17-Power-BI/06 Visualización/_Índice Visualización|Visualización]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **narrativa con datos** (data storytelling) es diseñar un informe para que **comunique una conclusión y lleve a una decisión**, no solo para mostrar números. Un buen informe responde "¿qué pasó?, ¿por qué importa? y ¿qué hacemos?". Las gráficas son el medio; la historia es el mensaje.

### Cómo funciona
Una estructura básica, inspirada en la narrativa clásica:

1. **Contexto**: ¿cuál es la situación y el objetivo? ("Meta trimestral: $15 M en ventas").
2. **Conflicto o hallazgo**: ¿qué cambió o qué no cuadra? ("Vamos 12 % abajo; la caída se concentra en la región Bajío").
3. **Explicación**: el detalle que lo sustenta (caída de clientes recurrentes, un producto sin inventario).
4. **Resolución / llamada a la acción**: ¿qué se recomienda? ("Reabastecer SKU 4471 y campaña de recuperación en Bajío").

Herramientas en Power BI para contarla:
- **Títulos que dicen la conclusión**, no la descripción. En vez de "Ventas por región", "Bajío explica 70 % de la caída". Los títulos pueden ser **dinámicos** con una medida (botón **fx** en el título), así la conclusión se actualiza con los filtros.
- **Jerarquía visual**: lo importante arriba a la izquierda; la página se lee en "Z" (ver [[Principios de diseño de informes]]).
- **Color de acento** solo en lo que sostiene la historia; el resto en gris.
- **Anotaciones**: cuadros de texto breves, líneas de referencia (meta, promedio) en el panel *Análisis*.
- **Secuencia**: páginas ordenadas o [[Marcadores y botones|marcadores]] en modo presentación para guiar paso a paso.
- **Texto generado**: el visual **narrativa de Copilot** genera un resumen en texto de la página o de visuales seleccionados, con notas al pie que señalan de qué visual sale cada afirmación, y se puede ajustar con indicaciones (tono, enfoque). Coexiste con el visual anterior de **narrativa inteligente** (smart narrative), con el que se puede alternar. Para crear la narrativa de Copilot en el Service, el workspace necesita capacidad Fabric de pago y que el administrador habilite Copilot (no funciona en SKU de prueba). El texto no se actualiza solo con los filtros: hay que refrescarlo.

```mermaid
flowchart LR
  C[Contexto] --> H[Hallazgo]
  H --> E[Explicación]
  E --> A[Acción recomendada]
```

### Ejemplo
Página "Cierre de octubre" para la dirección de Itzamna:

- Título dinámico:

```dax
Titulo cierre =
VAR v = [Var vs Meta %]
RETURN
    "Octubre cerró " & FORMAT ( ABS ( v ), "0%" ) &
    IF ( v >= 0, " arriba", " abajo" ) & " de la meta"
```

- Tarjetas: Ventas, Meta, Var vs Meta.
- Gráfico de barras por región con Bajío en color de acento y las demás en gris; línea de referencia de la meta.
- Cuadro de texto: "La caída de Bajío viene de clientes recurrentes (−18 %). Propuesta: campaña de recuperación en noviembre."
- Botón "Ver detalle" que lleva por drill through a la página de clientes de Bajío.

### Cuándo usarlo y cuándo no
- **Úsalo** en informes ejecutivos, presentaciones de resultados y cualquier página que se mire en una junta.
- En informes de **exploración** (analistas que buscan sus propias respuestas) la historia es menos rígida: prioriza filtros y flexibilidad, pero los títulos claros siguen ayudando.
- El texto de Copilot es un apoyo, no la conclusión: revísalo antes de presentarlo, porque se basa en lo que muestran los visuales y puede equivocarse.

### Errores comunes
1. **Mostrar todo "por si acaso"**: 15 gráficas sin mensaje. Elige la pregunta y quita lo que no la responde.
2. **Títulos descriptivos** ("Gráfico de ventas"): el usuario tiene que sacar la conclusión solo.
3. **Usar color para decorar** en lugar de para señalar.
4. **No decir qué hacer**: la historia termina en un dato, no en una acción.
5. **Confiar ciegamente en el texto generado** sin validar números.

### Para profundizar
- [Crear una narrativa con Copilot en Power BI (Microsoft Learn)](https://learn.microsoft.com/en-us/power-bi/create-reports/copilot-create-narrative)
- [Visual de narrativa inteligente](https://learn.microsoft.com/en-us/power-bi/visuals/power-bi-visualization-smart-narrative)
- [Storytelling with Data (sitio de Cole Nussbaumer Knaflic)](https://www.storytellingwithdata.com/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué estructura básica tiene una historia con datos?
> Contexto (qué pregunta), hallazgo principal, evidencia (visual que lo muestra) y acción o recomendación.

> [!question]- ¿Cómo ayudan los títulos al storytelling?
> Un título que dice la conclusión ("Las ventas de Equipo cayeron 9 % vs año anterior") en vez de describir ("Ventas por mes"). Se pueden hacer dinámicos con medidas.

> [!question]- ¿Qué es el visual de narrativa inteligente/Copilot narrativo?
> Un visual que genera texto resumen sobre los datos. Revisa siempre el texto y su disponibilidad: (verificar en la documentación).

