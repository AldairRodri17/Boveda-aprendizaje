---
proyecto: Conocimiento
tipo: concepto
nivel: avanzado
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
---
# DAX - medidas semiaditivas

Parte de: [[17-Power-BI/04 DAX/_Índice DAX|DAX]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **medida semiaditiva** se puede sumar en algunas dimensiones pero **no a lo largo del tiempo**. Ejemplos: existencias de inventario, saldo de una cuenta bancaria, número de empleados, cartera vencida. Sumar las existencias de los tres meses de un trimestre no da las existencias del trimestre: da un número sin sentido. Para el tiempo se usa otra agregación: último valor (cierre), primer valor (apertura) o promedio del periodo.

### Cómo funciona
Las tablas de este tipo guardan **fotos** (*snapshots*): una fila por fecha y producto con el valor en ese momento. La medida debe:
1. **Sumar** en las dimensiones normales (productos, almacenes): las existencias del día son la suma de todos los productos.
2. **Elegir una fecha** del periodo en el tiempo: la última con datos, la primera, o promediar los días.

Funciones útiles:
- `LASTDATE(Calendario[Fecha])` / `FIRSTDATE`: última o primera fecha **del calendario** visible, tenga o no datos.
- `LASTNONBLANK(Calendario[Fecha], expresión)` / `FIRSTNONBLANK`: última o primera fecha donde la expresión **no es BLANK**.
- `ENDOFMONTH`, `CLOSINGBALANCEMONTH`, `OPENINGBALANCEYEAR`: atajos de cierre y apertura de periodo.

**El problema de LASTDATE**: si la foto del inventario se tomó el viernes 28 de marzo y no hubo foto el 31 (fin de semana o día sin captura), `LASTDATE` en el contexto "marzo" devuelve el 31 y la medida queda en blanco. `LASTNONBLANK` busca hacia atrás el último día que sí tiene filas.

### Ejemplo
> [!example] Modelo de ejemplo de esta sección
> - `Productos`: P1 Café (Bebidas, Negro), P2 Molino (Equipo, Rojo), P3 Galletas (Alimentos, Café).
> - `Ventas` (Cantidad × PrecioUnitario, CostoUnitario): 1) 2024-01-15 P1 2×100 (60); 2) 2024-02-10 P2 1×250 (150); 3) 2024-02-20 P1 3×100 (60); 4) 2025-01-05 P3 5×40 (25); 5) 2025-02-14 P2 2×250 (150).
> - `Calendario` (Fecha, Año, NumMes, Mes, AñoMes, Trimestre) relacionado 1:* con `Ventas[Fecha]`; `Productos` 1:* con `Ventas[IdProducto]`.
> - Medida base: `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` → total **1 450**; Bebidas 500, Equipo 750, Alimentos 200; 2024 = 750, 2025 = 700.

Tabla `Inventario`(Fecha, IdProducto, Existencias), relacionada con Calendario y Productos, con estas fotos (no hubo foto el 31 de marzo):

| Fecha | P1 Café | P2 Molino | Total del día |
|---|---|---|---|
| 2025-01-31 | 20 | 5 | 25 |
| 2025-02-28 | 15 | 3 | 18 |
| 2025-03-28 | 12 | 4 | 16 |

```dax
Existencias suma (incorrecta) = SUM(Inventario[Existencias])
-- Primer trimestre 2025: 25 + 18 + 16 = 59  ← no significa nada

Existencias cierre =
CALCULATE(
    SUM(Inventario[Existencias]),
    LASTNONBLANK(Calendario[Fecha], CALCULATE(COUNTROWS(Inventario)))
)
-- T1-2025: foto del 28-mar → 16. Marzo: 16. Febrero: 18.

Existencias cierre LASTDATE =
CALCULATE(SUM(Inventario[Existencias]), LASTDATE(Calendario[Fecha]))
-- Marzo y T1: busca el 31-mar → BLANK

Existencias inicio =
CALCULATE(
    SUM(Inventario[Existencias]),
    FIRSTNONBLANK(Calendario[Fecha], CALCULATE(COUNTROWS(Inventario)))
)
-- T1-2025: foto del 31-ene → 25

Existencias promedio =
AVERAGEX(VALUES(Calendario[Fecha]), CALCULATE(SUM(Inventario[Existencias])))
-- T1-2025: solo 3 días tienen foto; AVERAGEX ignora los BLANK → (25 + 18 + 16) / 3 = 19.67
```

Por producto, la misma medida de cierre funciona: Café al cierre del T1 = 12, Molino = 4; el total de productos (16) sí es la suma, porque en la dimensión producto la medida es aditiva.

Una sutileza: `LASTNONBLANK` evaluado en el total de productos busca el último día con **cualquier** foto. Si cada producto tuviera fotos en fechas distintas, el total tomaría solo el último día global; para "sumar el último valor de cada producto" se itera: `SUMX(VALUES(Productos[IdProducto]), [Existencias cierre])`.

### Cuándo usarlo y cuándo no
- Cualquier dato de **estado** (saldo, existencias, plantilla, número de suscriptores activos) en contraste con datos de **flujo** (ventas, entradas, salidas), que sí son aditivos.
- Si tienes movimientos (entradas y salidas) en lugar de fotos, otra opción es calcular el saldo como acumulado de movimientos ([[DAX - totales acumulados]]), a costa de más cómputo.
- Evalúa si la foto debe ser diaria o mensual: fotos diarias de millones de productos crecen rápido.

### Errores comunes
- **Usar SUM a secas**: el total del trimestre o del año se infla.
- **Usar LASTDATE** cuando la última fecha del periodo no tiene foto: blank en meses y totales.
- **LASTNONBLANK con una medida pesada** como segundo argumento: itera fecha por fecha; usa `CALCULATE(COUNTROWS(Inventario))`, que es barato.
- **Promediar sin definir qué días cuentan**: ¿días con foto o todos los días del periodo? Son resultados distintos.

### Para profundizar
- [Semi-additive calculations (DAX Patterns)](https://www.daxpatterns.com/semi-additive-calculations/)
- [Función LASTNONBLANK](https://learn.microsoft.com/en-us/dax/lastnonblank-function-dax)
- [Funciones de inteligencia de tiempo](https://learn.microsoft.com/en-us/dax/time-intelligence-functions-dax)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Tabla `Inventario`(Fecha, IdProducto, Existencias) con fotos diarias. Medida de existencias al cierre del periodo.
> ```dax
> Existencias cierre =
> CALCULATE(SUM(Inventario[Existencias]),
>     LASTNONBLANK(Calendario[Fecha], CALCULATE(COUNTROWS(Inventario))))
> ```

> [!question]- Medida de existencias al inicio del periodo.
> ```dax
> Existencias inicio =
> CALCULATE(SUM(Inventario[Existencias]),
>     FIRSTNONBLANK(Calendario[Fecha], CALCULATE(COUNTROWS(Inventario))))
> ```

> [!question]- Promedio de existencias diarias del periodo (tabla Inventario del ejercicio anterior).
> ```dax
> Existencias promedio = AVERAGEX(VALUES(Calendario[Fecha]), CALCULATE(SUM(Inventario[Existencias])))
> ```
> AVERAGEX ignora los días en blanco (sin foto).

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es una medida semiaditiva?
> Una que se suma en algunas dimensiones pero no en el tiempo: existencias de inventario, saldo de cuentas, número de empleados.

> [!question]- ¿Por qué LASTDATE puede dar blank en el total de un mes?
> Si el último día del mes no tiene registro de inventario. Se usa `LASTNONBLANK(Calendario[Fecha], [medida])` para tomar el último día con datos.

> [!question]- Otra agregación semiaditiva común.
> Promedio de saldos en el periodo: `AVERAGEX(VALUES(Calendario[Fecha]), [Saldo])`.

