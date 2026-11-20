---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
---
# DAX - inteligencia de tiempo

Parte de: [[17-Power-BI/04 DAX/_Índice DAX|DAX]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **inteligencia de tiempo** es el conjunto de funciones DAX que calculan sobre periodos: acumulados (YTD, QTD, MTD), periodos anteriores, ventanas móviles y comparativos. Casi todas devuelven una **tabla de fechas** que se usa como filtro de `CALCULATE`. Los comparativos más usados tienen nota propia: [[DAX - YTD, mismo periodo del año anterior y comparativos]].

### Cómo funciona
**Requisitos de la versión clásica** (la que existe desde siempre):
1. Una tabla de calendario con **días contiguos** (sin huecos) que cubra años completos.
2. Relacionada con la tabla de hechos por una columna de tipo fecha.
3. **Marcada como tabla de fechas** (*Marcar como tabla de fechas*) o relacionada por una columna de tipo Date; y la opción *Fecha/hora automática* desactivada para no duplicar calendarios ocultos. Ver [[Tabla de calendario]].

**Funciones principales**

| Función | Devuelve (a partir de las fechas visibles) |
|---|---|
| `DATESYTD / DATESQTD / DATESMTD` | del inicio del año/trimestre/mes a la última fecha visible |
| `TOTALYTD(expr, fechas)` | atajo de `CALCULATE(expr, DATESYTD(fechas))` |
| `SAMEPERIODLASTYEAR(fechas)` | las mismas fechas, un año antes |
| `DATEADD(fechas, n, DAY/MONTH/QUARTER/YEAR)` | las fechas visibles desplazadas n periodos |
| `PARALLELPERIOD(fechas, n, MONTH/QUARTER/YEAR)` | el periodo **completo** desplazado |
| `PREVIOUSMONTH / PREVIOUSYEAR` | el mes/año completo anterior |
| `DATESINPERIOD(fechas, inicio, n, periodo)` | ventana móvil desde una fecha |
| `DATESBETWEEN(fechas, ini, fin)` | rango explícito |

**DATEADD vs PARALLELPERIOD**: si el contexto es febrero de 2025 completo, ambas a −1 año dan febrero de 2024. Pero si el contexto es solo del 1 al 14 de febrero, `DATEADD` desplaza esos 14 días, mientras que `PARALLELPERIOD(..., -1, MONTH)` devuelve **todo** el mes anterior. Con nivel año, `PARALLELPERIOD(..., -1, YEAR)` da el año anterior entero, aunque estés en febrero.

**Calendarios personalizados (inteligencia de tiempo basada en calendarios)**: Microsoft agregó una forma nueva en la que defines en el modelo uno o varios *calendarios* (metadatos que dicen qué columna es año, trimestre, mes, semana, día) y luego las funciones reciben el calendario: `TOTALYTD([Ventas $], 'Calendario Fiscal')`. Admite calendarios fiscales, 4-4-5, de 13 meses, cálculos por semana (`TOTALWTD`) y fechas no contiguas. Al consultarlo en noviembre de 2026, la documentación lo marca como **versión preliminar** que se activa en *Opciones → Características en versión preliminar → Enhanced DAX Time Intelligence*, y no se admite con conexiones dinámicas ni modelos compuestos.

### Ejemplo
> [!example] Modelo de ejemplo de esta sección
> - `Productos`: P1 Café (Bebidas, Negro), P2 Molino (Equipo, Rojo), P3 Galletas (Alimentos, Café).
> - `Ventas` (Cantidad × PrecioUnitario, CostoUnitario): 1) 2024-01-15 P1 2×100 (60); 2) 2024-02-10 P2 1×250 (150); 3) 2024-02-20 P1 3×100 (60); 4) 2025-01-05 P3 5×40 (25); 5) 2025-02-14 P2 2×250 (150).
> - `Calendario` (Fecha, Año, NumMes, Mes, AñoMes, Trimestre) relacionado 1:* con `Ventas[Fecha]`; `Productos` 1:* con `Ventas[IdProducto]`.
> - Medida base: `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` → total **1 450**; Bebidas 500, Equipo 750, Alimentos 200; 2024 = 750, 2025 = 700.

```dax
Ventas QTD = TOTALQTD([Ventas $], Calendario[Fecha])
-- Febrero 2025: enero (200) + febrero (500) = 700

Ventas año anterior completo =
CALCULATE([Ventas $], PARALLELPERIOD(Calendario[Fecha], -1, YEAR))
-- En febrero 2025 devuelve todo 2024 = 750

Ventas 3M =
CALCULATE(
    [Ventas $],
    DATESINPERIOD(Calendario[Fecha], MAX(Calendario[Fecha]), -3, MONTH)
)
-- Febrero 2025: diciembre 2024 a febrero 2025 = 0 + 200 + 500 = 700
-- Febrero 2024: diciembre 2023 a febrero 2024 = 200 + 550 = 750
```

### Cuándo usarlo y cuándo no
- **Funciones clásicas** para calendarios gregorianos o fiscales simples (año que cierra en junio: `DATESYTD(Calendario[Fecha], "6/30")`).
- **Calendarios personalizados** cuando el negocio usa semanas o calendarios 4-4-5; evalúa que el estado preliminar sea aceptable.
- **Filtros manuales** (`FILTER(ALL(Calendario), ...)` con columnas de año y mes) cuando la lógica no encaja en ninguna función, por ejemplo semanas ISO con la versión clásica.

### Errores comunes
- **Usar la fecha de la tabla de hechos** (`Ventas[Fecha]`) en vez del calendario: hay huecos y los filtros del visual no se quitan correctamente.
- **Calendario incompleto o con huecos**: resultados erróneos o errores en DATESYTD/SAMEPERIODLASTYEAR.
- **Dejar Fecha/hora automática activada**: aparecen jerarquías ocultas y comparaciones que no usan tu calendario.
- **No marcar la tabla de fechas** cuando la relación usa una columna entera (AAAAMMDD).
- **Comparar periodos incompletos** con periodos completos (ver la nota de YTD y comparativos).

### Para profundizar
- [Funciones de inteligencia de tiempo](https://learn.microsoft.com/en-us/dax/time-intelligence-functions-dax)
- [Inteligencia de tiempo en Power BI Desktop (incluye calendarios)](https://learn.microsoft.com/en-us/power-bi/transform-model/desktop-time-intelligence)
- [Función PARALLELPERIOD](https://learn.microsoft.com/en-us/dax/parallelperiod-function-dax)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Datos de ejemplo (para los ejercicios con resultado numérico): Productos P1 Café (Bebidas), P2 Molino (Equipo), P3 Galletas (Alimentos). Ventas: 1) 2024-01-15 P1 2×100; 2) 2024-02-10 P2 1×250; 3) 2024-02-20 P1 3×100; 4) 2025-01-05 P3 5×40; 5) 2025-02-14 P2 2×250. Medida base: `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` → total 1 450. Medida `Ventas trimestre a la fecha`. ¿Qué da en febrero 2025?
> ```dax
> Ventas QTD = TOTALQTD([Ventas $], Calendario[Fecha])
> ```
> T1-2025 hasta febrero: 200 + 500 = 700.

> [!question]- Medida de ventas del año anterior completo (no solo el mismo periodo). ¿Qué da en febrero 2025?
> ```dax
> Ventas año anterior completo = CALCULATE([Ventas $], PARALLELPERIOD(Calendario[Fecha], -1, YEAR))
> ```
> Todo 2024 → 750.

> [!question]- Medida de ventas de los últimos 3 meses (móvil) hasta la última fecha visible.
> ```dax
> Ventas 3M =
> CALCULATE([Ventas $],
>     DATESINPERIOD(Calendario[Fecha], MAX(Calendario[Fecha]), -3, MONTH))
> ```
> Feb-2025: dic-2024 a feb-2025 → 700.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué requisitos tiene la inteligencia de tiempo clásica?
> Una tabla de fechas con días contiguos y años completos, relacionada con los hechos y marcada como tabla de fechas (o relacionada por una columna de tipo fecha).

> [!question]- ¿Qué devuelven funciones como DATESYTD o SAMEPERIODLASTYEAR?
> Tablas de fechas desplazadas o acumuladas a partir de las fechas visibles; se usan como filtro en CALCULATE.

> [!question]- ¿Diferencia entre DATEADD y PARALLELPERIOD?
> DATEADD desplaza exactamente las fechas visibles (p. ej. −1 mes). PARALLELPERIOD devuelve el periodo completo desplazado (el mes/trimestre/año entero), sin importar si la selección era parcial.

> [!question]- ¿Qué es la inteligencia de tiempo basada en calendarios (calendarios personalizados)?
> Una funcionalidad más reciente para definir calendarios fiscales o 4-4-5 que las funciones de tiempo respetan. Disponibilidad y estado: (verificar en la documentación).

## Práctica

- [[Lab 6 - Medidas con CALCULATE y comparativos de tiempo en Power BI]]

