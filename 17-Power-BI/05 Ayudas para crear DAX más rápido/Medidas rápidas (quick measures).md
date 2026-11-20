---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
---
# Medidas rápidas (quick measures)

Parte de: [[17-Power-BI/05 Ayudas para crear DAX más rápido/_Índice Ayudas para crear DAX más rápido|Ayudas para crear DAX más rápido]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Las **medidas rápidas** son plantillas de Power BI Desktop que generan DAX para cálculos comunes sin escribir la fórmula: eliges el tipo de cálculo y arrastras los campos, y Power BI crea una medida normal con su código visible en la barra de fórmulas. No requieren Copilot ni licencias especiales y son una buena forma de **aprender** viendo el DAX que producen.

### Cómo funciona
1. En el panel *Datos*, clic derecho (o `...`) sobre una tabla o campo → *Nueva medida rápida*; también desde el menú desplegable de un valor en un visual.
2. En la ventana *Medidas rápidas*, eliges un cálculo en *Seleccionar un cálculo* y llenas los campos (valor base, categoría, fecha, filtro).
3. Power BI crea la medida en el panel Datos; al seleccionarla ves su DAX y puedes editarlo, renombrarla o eliminarla como cualquier otra.

Categorías disponibles según la documentación:
- **Agregado por categoría**: promedio, varianza, máximo, mínimo, promedio ponderado por categoría.
- **Filtros**: valor filtrado, diferencia o % de diferencia contra un valor filtrado, ventas de clientes nuevos.
- **Inteligencia de tiempo**: total del año, trimestre o mes hasta la fecha; cambio interanual, trimestral y mensual; promedio móvil.
- **Totales**: total acumulado, total por categoría (con o sin filtros aplicados).
- **Operaciones matemáticas**: suma, resta, multiplicación, división, % de diferencia, coeficiente de correlación.
- **Texto**: calificación con estrellas, lista concatenada de valores.

Limitaciones documentadas: necesitas poder modificar el modelo; en **DirectQuery** no se pueden crear las de inteligencia de tiempo; requieren tabla de fechas marcada para tiempo en modelos externos; y usan coma como separador de argumentos, por lo que fallan en idiomas de Desktop que usan coma decimal (con la configuración de México, que usa punto decimal, no hay problema).

### Ejemplo
> [!example] Modelo de ejemplo de esta sección
> - `Productos`: P1 Café (Bebidas, Negro), P2 Molino (Equipo, Rojo), P3 Galletas (Alimentos, Café).
> - `Ventas` (Cantidad × PrecioUnitario, CostoUnitario): 1) 2024-01-15 P1 2×100 (60); 2) 2024-02-10 P2 1×250 (150); 3) 2024-02-20 P1 3×100 (60); 4) 2025-01-05 P3 5×40 (25); 5) 2025-02-14 P2 2×250 (150).
> - `Calendario` (Fecha, Año, NumMes, Mes, AñoMes, Trimestre) relacionado 1:* con `Ventas[Fecha]`; `Productos` 1:* con `Ventas[IdProducto]`.
> - Medida base: `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` → total **1 450**; Bebidas 500, Equipo 750, Alimentos 200; 2024 = 750, 2025 = 700.

Medida rápida *Total acumulado* con valor base `Ventas $` y campo `Calendario[Fecha]`. El DAX generado se parece a esto (el texto exacto puede variar por versión):

```dax
Ventas $ total acumulado en Fecha =
CALCULATE(
    [Ventas $],
    FILTER(
        ALLSELECTED('Calendario'[Fecha]),
        ISONORAFTER('Calendario'[Fecha], MAX('Calendario'[Fecha]), DESC)
    )
)
-- Por mes: ene-24 200; feb-24 750; ene-25 950; feb-25 1 450
```

Medida rápida *Cambio interanual*: genera algo equivalente a

```dax
Ventas $ var. interanual =
VAR __AA = CALCULATE([Ventas $], DATEADD('Calendario'[Fecha], -1, YEAR))
RETURN IF(ISBLANK(__AA), BLANK(), DIVIDE([Ventas $] - __AA, __AA))
-- 2025 vs 2024: (700 − 750) / 750 = −6.67 %
```

**¿Por qué revisar el DAX que generan?**
- Usan `ALLSELECTED`, `FILTER` y nombres largos que quizá no son lo que tu caso necesita (por ejemplo, un acumulado que debería ignorar la segmentación de fechas, o reiniciarse cada año).
- Los nombres automáticos ("Ventas $ total acumulado en Fecha") no siguen tu convención.
- A veces el patrón es menos eficiente que uno escrito a mano o no considera meses futuros sin ventas.
- Es tu medida: si alguien pregunta cómo se calcula, debes poder explicarla.

### Cuándo usarlo y cuándo no
- **Úsalas** para aprender patrones (acumulados, comparativos), para prototipos rápidos y cuando no tienes Copilot.
- **Edita el resultado**: renombra, aplica formato, simplifica con variables y agrega descripción.
- **No las uses** como sustituto de entender el cálculo en medidas críticas; y para muchas variantes de tiempo considera [[Grupos de cálculo]].
- La versión con IA (sugerencias de Copilot) se reemplazó por Copilot en la vista de consultas DAX ([[Sugerencias de medidas rápidas con Copilot]]).

### Errores comunes
- **Dejar los nombres generados** y terminar con medidas imposibles de encontrar.
- **No marcar la tabla de calendario**: las de tiempo no aparecen o fallan.
- **Usar la fecha de la tabla de hechos** en el campo de fecha en lugar del calendario.
- **Duplicar medidas** rápidas casi iguales en vez de reutilizar una.

### Para profundizar
- [Usar medidas rápidas para cálculos comunes](https://learn.microsoft.com/en-us/power-bi/transform-model/desktop-quick-measures)
- [Medidas en Power BI Desktop](https://learn.microsoft.com/en-us/power-bi/transform-model/desktop-measures)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué son las medidas rápidas?
> Plantillas de Desktop que generan el DAX de cálculos comunes (YTD, variación, acumulado, promedio por categoría) a partir de campos que eliges.

> [!question]- ¿Por qué revisar el DAX que generan?
> Usan nombres largos y a veces patrones poco eficientes o que no siguen tus convenciones; además requieren tabla de fechas correcta. Son buenas para aprender el patrón y luego reescribirlo.

