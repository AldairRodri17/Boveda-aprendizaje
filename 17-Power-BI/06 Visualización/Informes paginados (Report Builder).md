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
# Informes paginados (Report Builder)

Parte de: [[17-Power-BI/06 Visualización/_Índice Visualización|Visualización]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **informe paginado** es un informe con diseño "pixel perfect", pensado para **imprimirse o exportarse** (PDF, Excel, Word…): facturas, estados de cuenta, listados de cientos de páginas, formatos oficiales. Viene de SQL Server Reporting Services (SSRS) y se crea con **Power BI Report Builder** (gratuito) en archivos `.rdl`. Se llama "paginado" porque crece a lo largo de tantas páginas como datos haya, a diferencia del informe interactivo, que es un lienzo fijo.

### Cómo funciona
1. **Origen de datos**: un modelo semántico de Power BI publicado, o conexiones directas (SQL Server, Azure SQL, Oracle, etc.; las locales vía [[Puerta de enlace de datos local (gateway)]]).
2. **Conjunto de datos (dataset)**: una consulta (SQL, procedimiento almacenado, o DAX/MDX con el diseñador de consultas si es un modelo de Power BI).
3. **Parámetros**: filtros que el usuario llena al abrir (fecha, cliente), que se pasan a la consulta.
4. **Regiones de datos**: tabla, matriz (tablix), lista, gráficos, con agrupaciones, encabezados que se repiten en cada página y saltos de página por grupo.
5. **Expresiones** estilo Visual Basic: `=Fields!Total.Value`, `=Globals!PageNumber`.
6. **Publicar** al Service y verlo, exportarlo o mandarlo por [[Suscripciones y alertas|suscripción]] (PDF, PPTX, XLSX, DOCX, CSV, XML).

**Licencias** (verificado en Microsoft Learn, nov 2026):
- Crear en Report Builder: no requiere licencia.
- Publicar en *Mi área de trabajo*: no requiere Pro/PPU; publicar en otras áreas de trabajo requiere **Pro o PPU**.
- Ver: usuarios con licencia gratuita pueden verlos si el workspace está en capacidad **Fabric F64+ o Premium P1+**; en workspaces sin capacidad, el lector necesita Pro o PPU.
- Ya no exigen capacidad Premium para existir: funcionan en workspaces Pro.

```mermaid
flowchart LR
  A[Report Builder .rdl] --> B[Dataset: SQL / proc / DAX]
  B --> C[Parámetros]
  C --> D[Tablix con grupos y saltos]
  D --> E[Publicar en workspace]
  E --> F[Exportar PDF/Excel o suscripción]
```

### Ejemplo
Estado de cuenta mensual por cliente para Pochteca.Nomisma, que hoy Wee.Reporteria genera en Excel a mano:

```sql
-- Consulta del dataset (parámetros @ClienteId y @Mes)
EXEC dbo.usp_EstadoCuenta_Obtener @ClienteId = @ClienteId, @Mes = @Mes;
```

En Report Builder: encabezado con logotipo y datos fiscales, tablix agrupado por factura con subtotales, pie con `="Página " & Globals!PageNumber & " de " & Globals!TotalPages`, y salto de página por cliente. Publicado en el workspace de Finanzas, una suscripción manda el PDF el día 1 de cada mes.

### Cuándo usarlo y cuándo no
- **Prefiérelo** cuando el entregable es un documento: facturas, listados completos para auditoría, formatos con diseño fijo, exportaciones de miles de filas a Excel con formato, impresiones.
- **Prefiere el interactivo** para exploración, tableros, análisis con segmentaciones y cruces entre visuales.
- Puedes combinarlos: el **visual de informe paginado** incrusta un paginado dentro de un informe interactivo y le pasa filtros.
- Para algo muy simple, el Service permite crear un paginado básico desde una tabla (*Crear informe paginado*), sin Report Builder.

### Errores comunes
1. **Usarlo como tablero**: no tiene interacciones cruzadas; se frustra el usuario.
2. **Consultas sin filtrar** que traen todo y luego filtran en el informe: pasa los parámetros a la consulta o procedimiento.
3. **Ancho del cuerpo + márgenes mayor que la página**: genera páginas en blanco al exportar a PDF. Cuerpo ≤ ancho de página − márgenes.
4. **Olvidar el gateway** para orígenes locales al publicar.
5. **No revisar licencias de los lectores**: si el workspace no está en capacidad, cada lector necesita Pro/PPU.

### Para profundizar
- [Informes paginados en Power BI: preguntas frecuentes (Microsoft Learn)](https://learn.microsoft.com/en-us/power-bi/paginated-reports/paginated-reports-faq)
- [Power BI Report Builder](https://learn.microsoft.com/en-us/power-bi/paginated-reports/paginated-reports-report-builder-power-bi)
- [¿Cuándo usar informes paginados?](https://learn.microsoft.com/en-us/power-bi/guidance/report-paginated-or-power-bi)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es un informe paginado?
> Un informe de diseño fijo, pensado para imprimir o exportar (PDF, Excel) con muchas páginas: facturas, listados, estados de cuenta. Se diseña con Power BI Report Builder (formato .rdl, heredado de SSRS).

> [!question]- ¿Cuándo preferirlo a un informe interactivo?
> Cuando se necesita todo el detalle paginado con formato exacto, encabezados por página y exportación fiel.

> [!question]- ¿Qué requisitos de licencia tiene publicarlos?
> Han variado con el tiempo (antes solo capacidad Premium). (Verificar en la página oficial.)

