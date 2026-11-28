---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
contenido: completo
---
# Cálculos visuales (visual calculations)

Parte de: [[17-Power-BI/04 DAX/_Índice DAX|DAX]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **cálculo visual** es una expresión DAX que se define **dentro de un visual** y se evalúa sobre la **matriz de datos que ese visual ya muestra** (sus filas, columnas y valores), no sobre el modelo. Simplifica los cálculos que dependen de la posición en el visual —acumulados, promedios móviles, porcentaje del padre, comparar con la fila anterior— que con medidas requieren CALCULATE y funciones de tiempo o ventana.

### Cómo funciona
1. Selecciona un visual y elige *Nuevo cálculo visual* (en la cinta *Inicio* o en el menú del visual). Se abre un editor con la matriz del visual, la barra de fórmulas y la vista previa.
2. Escribes la expresión referenciando los campos del visual por su nombre (por ejemplo `[Ventas $]`).
3. Las funciones propias se mueven sobre los **ejes** del visual (filas o columnas):
   - `RUNNINGSUM(expr)`: acumulado.
   - `MOVINGAVERAGE(expr, n)`: promedio móvil de n filas.
   - `PREVIOUS(expr)` / `NEXT(expr)`: valor de la fila anterior o siguiente.
   - `COLLAPSE(expr, eje)`: evalúa la expresión en el nivel superior de la jerarquía (útil para % del padre); `EXPAND` hace lo contrario.
   - `FIRST`, `LAST`, `ISATLEVEL` y funciones de ventana como `RANK(ROWS, ORDERBY(...))`.
4. Parámetro *Reset*: reinicia el cálculo por un nivel (por ejemplo, el acumulado se reinicia cada año).
5. Puedes **ocultar** los campos auxiliares del visual y mostrar solo el resultado.

Estado: según resúmenes de terceros de la actualización de Power BI de mayo de 2026, los cálculos visuales pasaron a **disponibilidad general** (Pendiente de confirmar en las notas oficiales de Microsoft); antes estuvieron mucho tiempo en versión preliminar. Si tu versión de Desktop es anterior, puede que necesites activarlos en *Opciones → Características en versión preliminar*.

### Ejemplo
> [!example] Modelo de ejemplo de esta sección
> - `Productos`: P1 Café (Bebidas, Negro), P2 Molino (Equipo, Rojo), P3 Galletas (Alimentos, Café).
> - `Ventas` (Cantidad × PrecioUnitario, CostoUnitario): 1) 2024-01-15 P1 2×100 (60); 2) 2024-02-10 P2 1×250 (150); 3) 2024-02-20 P1 3×100 (60); 4) 2025-01-05 P3 5×40 (25); 5) 2025-02-14 P2 2×250 (150).
> - `Calendario` (Fecha, Año, NumMes, Mes, AñoMes, Trimestre) relacionado 1:* con `Ventas[Fecha]`; `Productos` 1:* con `Ventas[IdProducto]`.
> - Medida base: `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` → total **1 450**; Bebidas 500, Equipo 750, Alimentos 200; 2024 = 750, 2025 = 700.

Gráfico de líneas con `Calendario[AñoMes]` en el eje y `[Ventas $]` en valores (solo meses con venta):

```dax
Acumulado = RUNNINGSUM([Ventas $])
-- ene-24 200; feb-24 750; ene-25 950; feb-25 1 450

Acumulado anual = RUNNINGSUM([Ventas $], HIGHESTPARENT)
-- con jerarquía Año > Mes en el eje: 2024: 200, 750; 2025: 200, 700

Vs mes anterior = [Ventas $] - PREVIOUS([Ventas $])
-- feb-24: 550 − 200 = 350

% del padre = DIVIDE([Ventas $], COLLAPSE([Ventas $], ROWS))
-- matriz Categoría > Producto: cada producto entre el subtotal de su categoría
```

Comparación con la medida equivalente del modelo:

```dax
Ventas acumuladas =
VAR UltFecha = MAX(Calendario[Fecha])
RETURN CALCULATE([Ventas $], Calendario[Fecha] <= UltFecha, REMOVEFILTERS(Calendario))
```

El cálculo visual es más corto y no requiere entender CALCULATE ni REMOVEFILTERS, pero solo existe en ese visual.

### Cuándo usarlo y cuándo no
- **Úsalo** para cálculos de presentación que dependen del orden o la jerarquía **de ese visual**: acumulados, móviles, % del padre, diferencias contra la fila anterior, rankings visuales.
- **Ventaja**: no ensucian el modelo con medidas de un solo uso, suelen ser más simples de escribir y pueden rendir mejor porque operan sobre datos ya agregados.
- **No lo uses** cuando la lógica deba **reutilizarse** en otros visuales, informes o en Excel: no se pueden copiar a otros visuales y no forman parte del modelo semántico. Para eso, una medida (ver [[DAX - totales acumulados]]).
- Tampoco cuando necesites relaciones del modelo dentro del cálculo: `RELATED`, `RELATEDTABLE` y `USERELATIONSHIP` no están disponibles.

### Errores comunes
- **Esperar que el resultado se exporte o se ancle**: las exportaciones de datos no incluyen los resultados, y no se pueden anclar a paneles ni usar con Publicar en la web.
- **Ordenar o filtrar por el cálculo visual**: no está soportado; el orden lo define el visual.
- **Olvidar que dependen del eje**: si cambias el orden o los campos del visual, el acumulado cambia de sentido.
- **Usarlos en visuales no compatibles** (segmentaciones, visuales de R/Python, visuales personalizados, árbol de descomposición, entre otros).

### Para profundizar
- [Usar cálculos visuales en Power BI Desktop](https://learn.microsoft.com/en-us/power-bi/transform-model/desktop-visual-calculations-overview)
- [Using RANK instead of RANKX (SQLBI, incluye versión de cálculo visual)](https://www.sqlbi.com/articles/using-rank-instead-of-rankx-in-dax/)
- [Función WINDOW](https://learn.microsoft.com/en-us/dax/window-function-dax)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué son los cálculos visuales?
> Cálculos DAX definidos sobre la matriz de datos de un visual concreto (no en el modelo), con funciones como `RUNNINGSUM`, `MOVINGAVERAGE`, `PREVIOUS`, `COLLAPSE`. Estado de disponibilidad: (verificar en la documentación).

> [!question]- ¿Ventaja frente a una medida?
> Más fáciles para acumulados, promedios móviles o % del padre, porque trabajan con lo que el visual ya muestra; no ensucian el modelo.

> [!question]- Limitación importante.
> Solo existen en ese visual: no se reutilizan en otros visuales ni informes.

> [!question]- Escribe un acumulado de Ventas $ en un cálculo visual.
> ```dax
> Acumulado = RUNNINGSUM([Ventas $])
> ```

