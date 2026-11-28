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
# Tabla de calendario

Parte de: [[17-Power-BI/03 Modelado de datos/_Índice Modelado de datos|Modelado de datos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **tabla de calendario** (o dimensión de fechas) es una tabla con **una fila por día**, sin huecos, que cubre todo el rango de fechas del modelo, con columnas para analizar por año, trimestre, mes, semana, día de la semana, festivos, año fiscal, etc. Se relaciona con cada columna de fecha de las tablas de hechos y es indispensable para que las funciones de **inteligencia de tiempo** de DAX (`TOTALYTD`, `SAMEPERIODLASTYEAR`, `DATEADD`) funcionen correctamente.

### Cómo funciona
**Requisitos** de una tabla de calendario válida:
1. Una columna de tipo **Fecha** (sin hora) con **valores únicos**.
2. **Sin huecos**: todos los días entre la primera y la última fecha.
3. **Años completos**: del 1 de enero del primer año al 31 de diciembre del último (o año fiscal completo).
4. Que cubra todas las fechas de **todas** las tablas de hechos.
5. **Marcarla como tabla de fechas** (*Herramientas de tabla > Marcar como tabla de fechas*, eligiendo la columna `Fecha`). Así DAX la reconoce y Power BI deja de usar jerarquías automáticas para ella.

**¿Por qué desactivar Fecha/hora automática?** (*Archivo > Opciones y configuración > Opciones > Carga de datos*, tanto en *Global* como en *Archivo actual*). Con ella activada, Power BI crea **una tabla de calendario oculta por cada columna de fecha** del modelo (Ventas[Fecha], Ventas[FechaEnvio], Clientes[FechaNacimiento]…), cada una del rango de su columna. Eso infla el modelo, impide compartir un mismo filtro de fecha entre tablas de hechos y te da jerarquías que no puedes personalizar (sin festivos, sin año fiscal, sin nombres en español). Con tu propio calendario tienes control total.

**Ordenar el nombre del mes**: la columna `Mes` ("enero", "febrero"…) se ordena alfabéticamente por defecto (abril, agosto, diciembre…). Selecciona la columna `Mes` > *Herramientas de columna* > **Ordenar por columna** > `NumMes`. Lo mismo para día de la semana con su número, y para `AñoMes` texto con una columna numérica `AñoMesNum` (202501).

### Ejemplo
Tabla calculada DAX para 2024–2025:

```dax
Calendario =
ADDCOLUMNS (
    CALENDAR ( DATE ( 2024, 1, 1 ), DATE ( 2025, 12, 31 ) ),
    "Año", YEAR ( [Date] ),
    "NumMes", MONTH ( [Date] ),
    "Mes", FORMAT ( [Date], "mmmm", "es-MX" ),
    "AñoMes", FORMAT ( [Date], "yyyy-mm" ),
    "Trimestre", "T" & QUARTER ( [Date] ),
    "NumDiaSemana", WEEKDAY ( [Date], 2 ),          // 1 = lunes ... 7 = domingo
    "EsFinDeSemana", WEEKDAY ( [Date], 2 ) > 5
)
```

`CALENDAR` devuelve una columna llamada `Date`; renómbrala a `Fecha` en la vista de tabla o usa `SELECTCOLUMNS`. Son 731 filas (2024 es bisiesto).

Variante **dinámica** que cubre los años completos de `Ventas[Fecha]`:

```dax
Calendario =
VAR Inicio = DATE ( YEAR ( MIN ( Ventas[Fecha] ) ), 1, 1 )
VAR Fin    = DATE ( YEAR ( MAX ( Ventas[Fecha] ) ), 12, 31 )
RETURN
    ADDCOLUMNS (
        CALENDAR ( Inicio, Fin ),
        "Año", YEAR ( [Date] ),
        "NumMes", MONTH ( [Date] ),
        "Mes", FORMAT ( [Date], "mmmm", "es-MX" )
    )
```

`CALENDARAUTO()` hace algo parecido, pero revisa **todas** las columnas de fecha del modelo (incluidas fechas de nacimiento de clientes) y puede generar rangos enormes.

Columna `EsFinDeSemana` agregada a una tabla existente (columna calculada):

```dax
EsFinDeSemana = WEEKDAY ( Calendario[Fecha], 2 ) >= 6
```

Alternativas: generarla en **Power Query** (consulta en blanco con `List.Dates`) o, mejor para proyectos corporativos, una tabla `DimFecha` en SQL Server con festivos oficiales de México y periodos fiscales, compartida por todos los modelos.

### Cuándo usarlo y cuándo no
- **Siempre** que el modelo tenga fechas y vayas a analizar por periodo.
- **DAX** para prototipos y modelos pequeños; **Power Query o SQL** cuando quieras festivos, columnas fiscales o reutilizarla en varios modelos.
- **Una sola tabla** para todas las fechas, salvo que necesites filtros independientes por rol ([[Dimensiones de rol múltiple]]).
- Si necesitas análisis por hora, crea una dimensión **Hora** aparte (1 440 minutos o 24 horas), no mezcles hora en el calendario.

### Errores comunes
- **No marcarla como tabla de fechas** y que la inteligencia de tiempo dé resultados raros con relaciones por columnas que no son de tipo fecha.
- **Rango incompleto**: ventas de diciembre que no aparecen porque el calendario termina en noviembre.
- **Relacionar con una columna `DateTime`** que trae horas: `2025-03-01 14:35` no coincide con `2025-03-01`; quita la hora en Power Query.
- **Olvidar el orden del mes**.
- **Dejar Fecha/hora automática activa**.

### Para profundizar
- [Crear tablas de fechas en Power BI Desktop](https://learn.microsoft.com/es-mx/power-bi/guidance/model-date-tables)
- [Fecha y hora automáticas en Power BI Desktop](https://learn.microsoft.com/es-mx/power-bi/transform-model/desktop-auto-date-time)
- [Establecer y usar tablas de fechas](https://learn.microsoft.com/es-mx/power-bi/transform-model/desktop-date-tables)

Practica en [[Lab 4 - Modelo estrella con tabla de calendario en Power BI]]. Relacionado: [[DAX - inteligencia de tiempo]].

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Escribe una tabla calculada DAX de calendario de 2024 a 2025 con Año, NumMes, Mes y AñoMes.
> ```dax
> Calendario =
> ADDCOLUMNS(
>     CALENDAR(DATE(2024, 1, 1), DATE(2025, 12, 31)),
>     "Año", YEAR([Date]),
>     "NumMes", MONTH([Date]),
>     "Mes", FORMAT([Date], "mmmm"),
>     "AñoMes", YEAR([Date]) * 100 + MONTH([Date])
> )
> ```
> `CALENDAR` devuelve una columna llamada `Date`. `FORMAT` usa la configuración regional del modelo para el nombre del mes.

> [!question]- Variante dinámica: que el calendario cubra los años completos de Ventas[Fecha].
> ```dax
> Calendario =
> VAR Ini = DATE(YEAR(MIN(Ventas[Fecha])), 1, 1)
> VAR Fin = DATE(YEAR(MAX(Ventas[Fecha])), 12, 31)
> RETURN ADDCOLUMNS(CALENDAR(Ini, Fin), "Año", YEAR([Date]))
> ```
> `CALENDARAUTO()` hace algo parecido pero revisa todas las columnas de fecha del modelo.

> [!question]- Agrega a Calendario una columna `EsFinDeSemana` en DAX.
> ```dax
> EsFinDeSemana = WEEKDAY(Calendario[Fecha], 2) >= 6
> ```
> Con tipo de retorno 2, lunes = 1 … domingo = 7.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué requisitos debe cumplir una tabla de calendario?
> Una fila por día, fechas contiguas sin huecos, cubrir años completos del rango de datos, columna de tipo fecha única; y marcarla como tabla de fechas.

> [!question]- ¿Por qué desactivar *Fecha/hora automática*?
> Crea una tabla oculta por cada columna de fecha, inflando el modelo, y no permite jerarquías ni atributos propios. Una tabla Calendario compartida es mejor.

> [!question]- ¿Cómo ordenas el nombre del mes correctamente?
> Con *Ordenar por columna*: Mes ordenado por NumMes. Si no, se ordena alfabéticamente (abril, agosto...).

## Práctica

- [[Lab 4 - Modelo estrella con tabla de calendario en Power BI]]

