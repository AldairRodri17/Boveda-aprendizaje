---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
contenido: completo
---
# Instalar Power BI Desktop y primer informe

Parte de: [[17-Power-BI/01 Fundamentos/_Índice Fundamentos|Fundamentos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Power BI Desktop** es la aplicación gratuita para Windows donde se construye todo: conexión a datos, Power Query, modelo, DAX e informe. No necesitas licencia para usarla; la licencia se necesita al publicar y compartir en el Service. No hay versión nativa para macOS: en Mac se usa una máquina virtual con Windows o un equipo Windows.

### Cómo funciona
**Formas de instalarlo** (según Microsoft Learn, revisado en noviembre de 2026):

1. **Microsoft Store** (recomendada para uso personal): se actualiza sola cada mes en segundo plano, descarga solo los cambios y **no requiere permisos de administrador**.
2. **Descarga directa** (`.exe` de 64 bits del Centro de descargas): útil cuando la empresa bloquea la Store o necesitas una versión específica. Las actualizaciones son manuales. La versión de 32 bits ya no tiene soporte.

No instales ambas en la misma máquina: no está soportado. Microsoft publica una versión nueva **cada mes** y solo da soporte a la más reciente; un `.pbix` guardado con una versión nueva no abre en una vieja. Requisitos mínimos: Windows 10 o posterior, .NET Framework 4.7.2+, WebView2, 2 GB de RAM libres (4 GB recomendados) y pantalla de al menos 1440×900. Tu PC con GTX 1660 Ti sobra; Desktop no usa la GPU para cálculo.

**Vistas principales** (barra izquierda):

- **Informe**: el lienzo donde arrastras visuales, filtros y segmentaciones.
- **Tabla** (antes "Datos"): ves las filas ya cargadas en el modelo y creas columnas calculadas.
- **Modelo**: relaciones, propiedades de columnas, jerarquías, roles de seguridad.

Además hay dos vistas de desarrollador: **Vista de consultas DAX** y **Vista TMDL** (editar el modelo como código). Y el **Editor de Power Query**, que se abre con *Transformar datos*.

### Ejemplo
Primer informe en 20 minutos con un Excel de ventas:

1. *Inicio > Obtener datos > Libro de Excel* y elige `Ventas2025.xlsx`.
2. En el navegador marca la **tabla** `tblVentas` (no la hoja) y pulsa **Transformar datos**.
3. En Power Query revisa tipos: `Fecha` como fecha, `Cantidad` como entero, `Importe` como decimal fijo. *Cerrar y aplicar*.
4. En la vista **Modelo** confirma que la tabla cargó.
5. Crea una medida en la vista Informe:

```dax
Ventas Totales = SUM ( tblVentas[Importe] )
```

6. Agrega una **tarjeta** con la medida, un **gráfico de columnas** por mes y una **segmentación** por región.
7. Guarda como `.pbix` (o como proyecto `.pbip` si lo vas a versionar en Azure DevOps).
8. *Publicar* a tu área de trabajo si tienes cuenta de trabajo o escuela.

### Cuándo usarlo y cuándo no
- **Desktop** es donde se crea el modelo y el diseño serio del informe. Hoy el Service también permite editar informes y algunos modelos en la web, útil para cambios rápidos o si usas Mac, pero la experiencia completa sigue en Desktop.
- **Store vs .exe**: en tu PC personal, la Store. En un equipo corporativo, lo que diga TI (a veces exigen MSI/EXE con versiones controladas).
- Si el destino es **Report Server**, instala la edición de Desktop para Report Server, no la normal.

### Errores comunes
- **Abrir el `.pbix` en otra PC y que la actualización falle.** El archivo guarda la ruta del origen (por ejemplo `C:\Users\tu\Documentos\Ventas.xlsx`) y **no guarda las credenciales**: esas viven en la configuración local de cada usuario (*Configuración de origen de datos*). En la otra PC la ruta no existe o no hay credenciales. Solución: parametrizar la ruta, usar SharePoint/OneDrive o una base de datos, y volver a capturar credenciales.
- **Tener dos instalaciones** (Store y `.exe`) y abrir archivos con la equivocada.
- **No actualizar**: un compañero con versión más nueva te manda un archivo y no abre.
- **Escala de Windows al 150 %** con pantalla pequeña: diálogos cortados. Desktop recomienda 100 %.
- **Dejar activada la fecha/hora automática** sin saberlo; crea tablas ocultas por cada columna de fecha (ver [[Tabla de calendario]]).

### Para profundizar
- [Obtener Power BI Desktop](https://learn.microsoft.com/es-mx/power-bi/fundamentals/desktop-get-the-desktop)
- [Introducción a Power BI Desktop](https://learn.microsoft.com/es-mx/power-bi/fundamentals/desktop-getting-started)
- [Novedades de Power BI Desktop (actualizaciones mensuales)](https://learn.microsoft.com/es-mx/power-bi/fundamentals/desktop-latest-update)

Practica con [[Lab 1 - Primer informe de Power BI desde Excel o CSV]].

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cuáles son las formas de instalar Power BI Desktop y cuál conviene?
> Desde Microsoft Store (se actualiza sola) o con el instalador descargable (útil cuando la Store está bloqueada o se necesita una versión fija). Para aprender, la Store es lo más cómodo. Solo existe para Windows.

> [!question]- ¿Qué tres vistas principales tiene Desktop?
> **Informe** (diseñar páginas y visuales), **Tabla/Datos** (ver filas del modelo) y **Modelo** (relaciones y propiedades). Además están la vista de consultas DAX y, en algunos casos, la vista TMDL.

> [!question]- Error común: abrir un .pbix en otra PC y que falle la actualización. ¿Por qué?
> El .pbix guarda la ruta del origen (p. ej. `C:\Users\yo\ventas.xlsx`). En otra PC esa ruta no existe. Se corrige en *Transformar datos → Configuración de origen de datos* o, mejor, usando un parámetro para la ruta.

## Práctica

- [[Lab 1 - Primer informe de Power BI desde Excel o CSV]]

