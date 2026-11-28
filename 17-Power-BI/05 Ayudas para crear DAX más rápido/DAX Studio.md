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
# DAX Studio

Parte de: [[17-Power-BI/05 Ayudas para crear DAX más rápido/_Índice Ayudas para crear DAX más rápido|Ayudas para crear DAX más rápido]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**DAX Studio** es una herramienta gratuita y de código abierto para Windows que sirve para **escribir, ejecutar y analizar consultas DAX** contra modelos tabulares: Power BI Desktop, modelos publicados por XMLA (Premium, PPU o Fabric), Analysis Services y Power Pivot. Es la herramienta estándar para **medir el rendimiento** de DAX. Al consultar su sitio en noviembre de 2026, la versión publicada era la 3.6.2 (instalador y versión portátil; también por winget o Chocolatey).

### Cómo funciona
- **Conectar**: desde Power BI Desktop, *Herramientas externas → DAX Studio* (se registra al instalarlo) o desde DAX Studio eligiendo el modelo de Desktop abierto. Para modelos publicados, la cadena de conexión del punto de conexión XMLA del área de trabajo.
- **Escribir y ejecutar** consultas `EVALUATE` con IntelliSense, formato (usa DAX Formatter) y salida a cuadrícula, archivo o portapapeles; también exporta tablas completas (CSV, Parquet, entre otros).
- **Server Timings**: muestra cuánto tardó la consulta y cómo se repartió entre el **motor de almacenamiento** (SE, VertiPaq, multihilo, con caché) y el **motor de fórmulas** (FE, un hilo). Lista cada subconsulta al SE (en pseudo-SQL llamado *xmSQL*), sus filas y duración, y si hubo *callbacks* al FE. Si el FE domina o hay cientos de consultas al SE, el problema suele estar en el DAX; si una consulta al SE es enorme, en el modelo o la cardinalidad.
- **Query Plan**: plan lógico y físico de la consulta, para análisis avanzado.
- **View Metrics (VertiPaq Analyzer)**: estadísticas del modelo: tamaño por tabla y columna, cardinalidad, diccionarios, relaciones y su integridad referencial. Responde "¿qué columna pesa más?" y "¿hay claves sin coincidencia?". Se puede exportar a un archivo `.vpax` para compartir sin datos.
- **Clear cache / Benchmark**: borra la caché para medir en frío y ejecuta varias veces para obtener tiempos estables.

### Ejemplo
> [!example] Modelo de ejemplo de esta sección
> - `Productos`: P1 Café (Bebidas, Negro), P2 Molino (Equipo, Rojo), P3 Galletas (Alimentos, Café).
> - `Ventas` (Cantidad × PrecioUnitario, CostoUnitario): 1) 2024-01-15 P1 2×100 (60); 2) 2024-02-10 P2 1×250 (150); 3) 2024-02-20 P1 3×100 (60); 4) 2025-01-05 P3 5×40 (25); 5) 2025-02-14 P2 2×250 (150).
> - `Calendario` (Fecha, Año, NumMes, Mes, AñoMes, Trimestre) relacionado 1:* con `Ventas[Fecha]`; `Productos` 1:* con `Ventas[IdProducto]`.
> - Medida base: `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` → total **1 450**; Bebidas 500, Equipo 750, Alimentos 200; 2024 = 750, 2025 = 700.

Flujo para un visual lento:

1. En Desktop, *Analizador de rendimiento* → copiar la consulta del visual.
2. En DAX Studio, pegar la consulta, activar *Server Timings* y *Clear Cache then Run*.
3. Revisar el total y el reparto SE/FE; ver en la pestaña de Server Timings las subconsultas.
4. Probar una versión alternativa de la medida con `DEFINE MEASURE` en la misma consulta y comparar.

```dax
DEFINE
    -- Versión que itera la tabla de hechos
    MEASURE Ventas[Ventas rojo A] =
        SUMX(FILTER(Ventas, RELATED(Productos[Color]) = "Rojo"),
             Ventas[Cantidad] * Ventas[PrecioUnitario])
    -- Versión que filtra la dimensión
    MEASURE Ventas[Ventas rojo B] =
        CALCULATE([Ventas $], Productos[Color] = "Rojo")
EVALUATE
SUMMARIZECOLUMNS(Calendario[Año], "A", [Ventas rojo A], "B", [Ventas rojo B])
-- 2024: 250 / 250; 2025: 500 / 500
```

Ambas dan lo mismo; en un modelo con millones de filas, Server Timings mostraría si la versión A genera más trabajo en el FE. Con *View Metrics* verías además que, en tablas reales, columnas como un `IdVenta` de texto o una fecha con hora suelen ser las que más memoria ocupan.

### Cuándo usarlo y cuándo no
- **Úsalo** para diagnosticar rendimiento, revisar el tamaño del modelo, ejecutar consultas largas, exportar datos del modelo y probar medidas alternativas.
- Para pruebas rápidas de medidas, la [[Vista de consultas DAX (DAX query view)]] integrada puede bastar; DAX Studio aporta las métricas del motor que la vista de consultas no muestra.
- Para editar el modelo (medidas, propiedades, grupos de cálculo), usa [[Tabular Editor]] o Desktop; DAX Studio está pensado para consultar y medir.

### Errores comunes
- **Medir con caché caliente**: el segundo resultado siempre es más rápido; usa *Clear cache then run*.
- **Comparar consultas distintas**: copia la consulta exacta del visual, con sus filtros.
- **Fijarse solo en el tiempo total** sin ver el reparto SE/FE.
- **Compartir el `.pbix` para que otro analice el tamaño** cuando basta un `.vpax` (no contiene datos).

### Para profundizar
- [Sitio oficial de DAX Studio](https://daxstudio.org/)
- [DAX Studio en SQLBI Tools](https://www.sqlbi.com/tools/dax-studio/)
- [Herramientas externas en Power BI Desktop](https://learn.microsoft.com/en-us/power-bi/transform-model/desktop-external-tools)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es DAX Studio?
> Herramienta gratuita para escribir y ejecutar consultas DAX contra un modelo (Desktop abierto, Service vía XMLA, SSAS), con métricas de rendimiento.

> [!question]- ¿Qué muestra *Server Timings*?
> Tiempo total, tiempo en Storage Engine vs Formula Engine, y las consultas internas (xmSQL) del SE; sirve para encontrar cuellos de botella.

> [!question]- ¿Para qué sirve *View Metrics* (VertiPaq Analyzer)?
> Ver tamaño de tablas y columnas, cardinalidad y codificación, para detectar qué ocupa más memoria.

## Práctica

- [[Lab 7 - Consultas DAX con la vista de consultas y DAX Studio]]

