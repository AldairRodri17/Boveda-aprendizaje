---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Power BI
herramienta: Power BI Desktop
tags: [power-bi, laboratorio]
contenido: completo
---
# Lab 8 - Informe interactivo con marcadores, drill through y tooltips

Parte de: [[17-Power-BI/06 Visualización/_Índice Visualización|Visualización]]

**Cubre:** [[Principios de diseño de informes]] · [[Interacciones entre visuales]] · [[Información sobre herramientas (tooltips)]] · [[Drill down y drill through]] · [[Marcadores y botones]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Construir una página resumen y una de detalle conectadas por drill through, con un tooltip de página y un botón que alterna gráfico/tabla.

## Pasos

1. Página *Resumen*: tarjetas (Ventas $, Var vs AA %), barras por Categoria y línea por mes.
2. Edita interacciones para que la tarjeta de Ventas $ no se filtre al hacer clic en la línea.
3. Página *Detalle producto*: tabla de ventas por fecha; agrega Productos[Producto] a *Obtener detalles*.
4. Página *Tooltip*: tipo información sobre herramientas, con una tarjeta y mini barras por año; asígnala al gráfico de barras.
5. En Resumen, coloca una tabla encima del gráfico de barras (mismo tamaño) y crea dos marcadores (Gráfico / Tabla) usando el panel Selección, sin guardar *Datos*.
6. Agrega dos botones con acción Marcador.
7. Revisa alineación, títulos que digan la conclusión y no más de 6 visuales por página.

## Criterios de terminado

- [ ] Clic derecho en una barra → *Obtener detalles* lleva a la página de detalle filtrada.
- [ ] Al pasar el cursor por una barra aparece el tooltip de página con datos solo de esa categoría.
- [ ] Los botones alternan gráfico y tabla sin borrar la segmentación seleccionada.
- [ ] La tarjeta de Ventas $ no cambia al hacer clic en la línea.

## Solución

> [!success]- Marcadores que no reinician filtros
> En el panel Marcadores → *...* de cada marcador → desmarca **Datos**; deja *Visualización* y *Página actual*.

> [!success]- Drill through no aparece
> El visual origen debe usar el mismo campo (Producto) o uno relacionado; agrega Producto al eje o a la tabla del visual origen.

## Notas personales

_Qué me costó, errores que cometí, dudas._
