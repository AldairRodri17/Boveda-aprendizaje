---
proyecto: Conocimiento
tipo: concepto
nivel: avanzado
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
contenido: completo
---
# Tabular Editor

Parte de: [[17-Power-BI/05 Ayudas para crear DAX más rápido/_Índice Ayudas para crear DAX más rápido|Ayudas para crear DAX más rápido]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Tabular Editor** es un editor de modelos semánticos (tabulares) para Power BI y Analysis Services. En lugar de hacer clic objeto por objeto en Desktop, ves el modelo completo como un árbol (tablas, columnas, medidas, relaciones, roles, grupos de cálculo, perspectivas) y lo editas en bloque, con scripts y reglas de calidad. Existen dos productos:
- **Tabular Editor 2 (TE2)**: **gratuito y de código abierto**; su fabricante indica que sigue recibiendo correcciones.
- **Tabular Editor 3 (TE3)**: **comercial**, con ediciones *Desktop*, *Business*, *Enterprise* y *Companywide*, suscripción por usuario mensual o anual. Precios: Pendiente de confirmar en la página de licencias (no aparecían en el texto consultado en noviembre de 2026).

### Cómo funciona
- **Conexión**: como herramienta externa desde Power BI Desktop (edita el modelo abierto), contra el punto de conexión XMLA de un área de trabajo Premium/PPU/Fabric, contra Analysis Services, o sobre archivos (`.bim`, carpetas, y los formatos de los proyectos PBIP/TMDL) sin conexión.
- **Edición en bloque**: seleccionar 30 medidas y cambiarles el formato, la carpeta de visualización o la descripción de una vez; renombrar con buscar/reemplazar.
- **Scripts en C#**: automatizar tareas repetitivas (crear medidas YTD/AA para cada medida base, generar descripciones, ocultar claves). Para un desarrollador .NET es natural.
- **Grupos de cálculo** y otros objetos que Desktop no siempre expone con comodidad (perspectivas, particiones, propiedades avanzadas).
- **Best Practice Analyzer (BPA)**: evalúa el modelo contra un conjunto de **reglas** (archivo JSON) y marca incumplimientos: columnas de punto flotante, relaciones bidireccionales, medidas sin formato, columnas sin uso, uso de `/` en vez de DIVIDE, falta de descripciones. Muchas reglas pueden corregirse automáticamente. Microsoft publica un conjunto de reglas de referencia en GitHub, y se pueden escribir reglas propias. Puede ejecutarse en línea de comandos, útil en una canalización de Azure DevOps.

**TE2 vs TE3** según el fabricante: ambos editan los mismos objetos y propiedades y usan el mismo formato de carpetas. TE3 agrega un editor DAX nuevo con IntelliSense y formato avanzado, **depurador de DAX**, consultas y vista previa de datos integradas, diagramas, interfaz moderna (alta resolución, varios monitores) e integración con DAX Optimizer, entre otras funciones. Algunas capacidades dependen de la edición (por ejemplo, el tipo de servidor al que se conecta por XMLA).

### Ejemplo
> [!example] Modelo de ejemplo de esta sección
> - `Productos`: P1 Café (Bebidas, Negro), P2 Molino (Equipo, Rojo), P3 Galletas (Alimentos, Café).
> - `Ventas` (Cantidad × PrecioUnitario, CostoUnitario): 1) 2024-01-15 P1 2×100 (60); 2) 2024-02-10 P2 1×250 (150); 3) 2024-02-20 P1 3×100 (60); 4) 2025-01-05 P3 5×40 (25); 5) 2025-02-14 P2 2×250 (150).
> - `Calendario` (Fecha, Año, NumMes, Mes, AñoMes, Trimestre) relacionado 1:* con `Ventas[Fecha]`; `Productos` 1:* con `Ventas[IdProducto]`.
> - Medida base: `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` → total **1 450**; Bebidas 500, Equipo 750, Alimentos 200; 2024 = 750, 2025 = 700.

Script C# en Tabular Editor para crear, por cada medida seleccionada, su versión YTD y AA (alternativa a [[Grupos de cálculo]] cuando se quiere medidas físicas):

```csharp
// Selecciona [Ventas $] y [Costo $] en el árbol y ejecuta el script
foreach (var m in Selected.Measures)
{
    var ytd = m.Table.AddMeasure(
        m.Name + " YTD",
        "CALCULATE(" + m.DaxObjectName + ", DATESYTD(Calendario[Fecha]))",
        m.DisplayFolder);
    ytd.FormatString = m.FormatString;
    ytd.Description = "Acumulado del año de " + m.Name;

    var aa = m.Table.AddMeasure(
        m.Name + " AA",
        "CALCULATE(" + m.DaxObjectName + ", SAMEPERIODLASTYEAR(Calendario[Fecha]))",
        m.DisplayFolder);
    aa.FormatString = m.FormatString;
}
```

Resultado esperado en febrero de 2025: `Ventas $ YTD` = 700 y `Ventas $ AA` = 550; `Costo $ YTD` = 425 y `Costo $ AA` = 330.

Una regla BPA típica (simplificada) para exigir formato en medidas:

```json
{
  "ID": "MEDIDA_SIN_FORMATO",
  "Name": "Las medidas deben tener cadena de formato",
  "Scope": "Measure",
  "Severity": 2,
  "Expression": "string.IsNullOrWhitespace(FormatString)"
}
```

### Cuándo usarlo y cuándo no
- **Úsalo** cuando el modelo pasa de unas cuantas medidas: cambios masivos, grupos de cálculo, revisión de calidad con BPA, trabajo con PBIP en Git.
- **TE2** basta para aprender, scripts y BPA; **TE3** conviene si pasas muchas horas al día modelando y valoras el editor, el depurador y la productividad (y alguien paga la licencia).
- Para medir rendimiento de consultas, usa [[DAX Studio]]; para diseño de informes (visuales), Desktop.

### Errores comunes
- **Editar un modelo de Desktop sin respaldo**: guarda o usa control de versiones antes de scripts masivos.
- **Cambios por XMLA a un modelo del Service** que luego impiden descargar el `.pbix`: planea el flujo de despliegue.
- **Activar todas las reglas BPA a ciegas**: ajusta la severidad a tu contexto.
- **Confundir las ediciones de TE3** al comprar: revisa qué servidores admite cada una.

### Para profundizar
- [Documentación de Tabular Editor](https://docs.tabulareditor.com/)
- [Ediciones y licencias de Tabular Editor 3](https://tabulareditor.com/licensing)
- [Reglas de buenas prácticas de Microsoft (GitHub)](https://github.com/microsoft/Analysis-Services/tree/master/BestPracticeRules)
- [Herramientas externas en Power BI Desktop](https://learn.microsoft.com/en-us/power-bi/transform-model/desktop-external-tools)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Para qué se usa Tabular Editor?
> Editar el modelo de forma masiva y precisa: medidas, carpetas, traducciones, grupos de cálculo, OLS, perspectivas, scripts C# y Best Practice Analyzer.

> [!question]- ¿Diferencia entre Tabular Editor 2 y 3?
> TE2 es gratuito y de código abierto; TE3 es comercial con editor más completo y funciones extra (licencia: verificar en la página oficial).

> [!question]- ¿Qué es Best Practice Analyzer?
> Un conjunto de reglas que revisa el modelo (medidas sin formato, columnas sin usar, relaciones bidireccionales, etc.) y sugiere correcciones.

