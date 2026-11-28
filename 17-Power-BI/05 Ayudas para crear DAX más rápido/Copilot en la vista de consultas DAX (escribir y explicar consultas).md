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
# Copilot en la vista de consultas DAX (escribir y explicar consultas)

Parte de: [[17-Power-BI/05 Ayudas para crear DAX más rápido/_Índice Ayudas para crear DAX más rápido|Ayudas para crear DAX más rápido]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Es la integración de **Copilot (Microsoft Fabric Copilot)** dentro de la [[Vista de consultas DAX (DAX query view)]]: describes en lenguaje natural lo que quieres ("ventas y margen por categoría en 2025, de mayor a menor") y Copilot propone una **consulta DAX**, que puede incluir medidas temporales con `DEFINE MEASURE`. También **explica** consultas existentes y temas de DAX. No sustituye saber DAX: acelera escribir el primer borrador y entender código ajeno.

### Cómo funciona
Según la documentación de Microsoft Learn (consultada en noviembre de 2026):
- **Abrir**: en una pestaña de consulta, Ctrl+I o el botón de Copilot. En algunas versiones de Desktop hay que activar la característica en *Opciones → Características en versión preliminar*.
- **Escribir**: describes la consulta, Copilot genera el DAX, lo **ejecutas** para ver resultados y eliges *Mantener consulta* para dejarla en la pestaña. Si el DAX generado tiene errores de sintaxis, Copilot reintenta una vez; si sigue mal, te pide reformular.
- **Modificar**: seleccionas una consulta y pides un cambio ("agrega una columna con el ticket promedio"); una vista de diferencias marca lo que cambió.
- **Explicar**: *Explicar esta consulta* da una explicación breve (con *Mostrar más*). *Explicar un tema de DAX* responde preguntas como "¿para qué sirve SUMMARIZECOLUMNS?".
- **Qué ve Copilot**: los **metadatos** del modelo (nombres de tablas, columnas, medidas y sus descripciones) y todo el texto de la ventana de consulta; en modo Import puede usar valores mínimos y máximos de ejemplo, pero no lee el resto de los datos. Solo usa los primeros 200 caracteres de cada descripción, y con modelos muy grandes (por ejemplo, más de 500 tablas, 5 000 medidas o 10 000 columnas) puede trabajar con metadatos reducidos o no estar disponible.
- **Requisitos**: el área de trabajo debe estar en una capacidad Fabric de pago (F2 o superior), Premium (P1 o superior) o tener una capacidad de Copilot de Fabric; ver [[Requisitos y licencias para usar Copilot en Power BI]].

### Ejemplo
> [!example] Modelo de ejemplo de esta sección
> - `Productos`: P1 Café (Bebidas, Negro), P2 Molino (Equipo, Rojo), P3 Galletas (Alimentos, Café).
> - `Ventas` (Cantidad × PrecioUnitario, CostoUnitario): 1) 2024-01-15 P1 2×100 (60); 2) 2024-02-10 P2 1×250 (150); 3) 2024-02-20 P1 3×100 (60); 4) 2025-01-05 P3 5×40 (25); 5) 2025-02-14 P2 2×250 (150).
> - `Calendario` (Fecha, Año, NumMes, Mes, AñoMes, Trimestre) relacionado 1:* con `Ventas[Fecha]`; `Productos` 1:* con `Ventas[IdProducto]`.
> - Medida base: `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` → total **1 450**; Bebidas 500, Equipo 750, Alimentos 200; 2024 = 750, 2025 = 700.

Indicación (*prompt*): "Escribe una consulta que muestre por categoría las ventas, el número de líneas y el ticket promedio, ordenada de mayor a menor venta."

Respuesta típica (revísala antes de aceptarla):

```dax
DEFINE
    MEASURE Ventas[Num lineas] = COUNTROWS(Ventas)
    MEASURE Ventas[Ticket promedio] = DIVIDE([Ventas $], [Num lineas])
EVALUATE
SUMMARIZECOLUMNS(
    Productos[Categoria],
    "Ventas", [Ventas $],
    "Lineas", [Num lineas],
    "Ticket", [Ticket promedio]
)
ORDER BY [Ventas] DESC
-- Equipo 750, 2, 375; Bebidas 500, 2, 250; Alimentos 200, 1, 200
```

Antes de guardar `Ticket promedio` en el modelo con *Actualizar modelo*: compara los números con un cálculo a mano (aquí 750 / 2 = 375), revisa que use `DIVIDE`, que el nombre siga tu convención y que se comporte bien en el total (`EVALUATE { [Ticket promedio] }` → 1 450 / 5 = 290).

**Qué ayuda a que genere mejor DAX**
- Nombres claros en tablas, columnas y medidas (`Ventas[Fecha]`, no `Tabla1[Col3]`).
- **Descripciones** en medidas y columnas clave (las primeras 200 letras cuentan).
- Un modelo estrella con relaciones correctas y una tabla de calendario marcada.
- Indicaciones precisas: qué medida, qué agrupación, qué filtro, qué orden.
- Ocultar columnas técnicas que no deberían usarse.

### Cuándo usarlo y cuándo no
- **Úsalo** para borradores de consultas, recordar sintaxis, explorar un modelo que no conoces y entender DAX heredado.
- **Úsalo con cuidado** en medidas con contexto delicado (transición de contexto, totales no aditivos, inteligencia de tiempo con calendarios fiscales): ahí los errores son sutiles.
- **No lo uses** como fuente de verdad: Copilot puede no conocer funciones DAX recientes y sus explicaciones pueden ser incorrectas.
- Sin capacidad compatible no está disponible; la alternativa gratuita es escribir la consulta tú y apoyarte en DAX Guide y DAX Studio.

### Errores comunes
- **Aceptar sin ejecutar**: siempre ejecuta y compara con valores conocidos.
- **Pensar que Copilot "vio los datos"**: trabaja sobre metadatos; si el nombre de una columna es ambiguo, adivina.
- **Dejar texto confidencial en la ventana**: Copilot recibe todo el texto de la consulta, no solo lo seleccionado.
- **Guardar medidas con nombres genéricos** que Copilot propuso (`Measure 1`): renómbralas y documéntalas.

### Para profundizar
- [Usar Copilot para escribir y explicar consultas DAX](https://learn.microsoft.com/en-us/power-bi/transform-model/dax-query-copilot-create)
- [Copilot para DAX (referencia DAX)](https://learn.microsoft.com/en-us/dax/dax-copilot)
- [Habilitar Copilot de Fabric para Power BI](https://learn.microsoft.com/en-us/power-bi/create-reports/copilot-enable-power-bi)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Para qué se puede usar Copilot en la vista de consultas DAX?
> Pedir en lenguaje natural una consulta o medida ("ventas por categoría del año pasado"), y pedir que explique una consulta existente. La disponibilidad depende de licencia, región y configuración del inquilino; no la des por hecha (verificar en la página oficial).

> [!question]- ¿Qué ayuda a que Copilot genere mejor DAX?
> Un modelo con nombres claros de tablas, columnas y medidas, descripciones, relaciones correctas y columnas innecesarias ocultas.

> [!question]- ¿Qué debes hacer antes de guardar una medida sugerida?
> Ejecutarla, comparar contra un cálculo conocido (un total que puedas verificar en el origen) y leer el código para entender filtros y contexto.

