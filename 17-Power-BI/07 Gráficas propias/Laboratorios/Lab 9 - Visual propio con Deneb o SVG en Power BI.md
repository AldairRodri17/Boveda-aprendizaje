---
proyecto: Conocimiento
tipo: laboratorio
nivel: avanzado
progreso: pendiente
estado: borrador
area: Power BI
herramienta: Power BI Desktop + Deneb (AppSource, gratuito)
tags: [power-bi, laboratorio]
contenido: completo
---
# Lab 9 - Visual propio con Deneb o SVG en Power BI

Parte de: [[17-Power-BI/07 Gráficas propias/_Índice Gráficas propias|Gráficas propias]]

**Cubre:** [[Visuales personalizados - panorama de opciones]] · [[Deneb (Vega y Vega-Lite)]] · [[SVG dentro de medidas DAX]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Crear un gráfico de barras con etiquetas en Deneb y una barra de progreso SVG dentro de una tabla.

## Pasos

1. Importa Deneb desde *Obtener más objetos visuales* (AppSource).
2. Agrega Deneb al lienzo con Productos[Categoria] y [Ventas $]; elige Vega-Lite y una plantilla vacía.
3. Escribe una especificación de barras horizontales ordenadas, con etiquetas y tooltip.
4. Activa la interactividad (selección cruzada) en las opciones de Deneb y prueba que filtra otro visual.
5. Crea la medida `Barra SVG` con base en `% del total` y márcala como *URL de imagen*.
6. Ponla en una tabla junto a Categoria y Ventas $.

## Criterios de terminado

- [ ] El visual de Deneb muestra 3 barras ordenadas: Equipo, Bebidas, Alimentos, con etiquetas 750, 500, 200.
- [ ] Clic en una barra de Deneb filtra la tabla.
- [ ] La tabla muestra una barra azul proporcional al % del total en cada fila.
- [ ] Guardaste la especificación JSON en esta carpeta.

## Solución

> [!success]- Especificación Vega-Lite
> ```json
> {
>   "data": {"name": "dataset"},
>   "encoding": {
>     "y": {"field": "Categoria", "type": "nominal", "sort": "-x", "title": null},
>     "x": {"field": "Ventas $", "type": "quantitative", "title": "Ventas"}
>   },
>   "layer": [
>     {"mark": {"type": "bar", "tooltip": true}},
>     {"mark": {"type": "text", "align": "left", "dx": 3},
>      "encoding": {"text": {"field": "Ventas $", "format": ",.0f"}}}
>   ]
> }
> ```

> [!success]- Medida SVG
> ```dax
> Barra SVG =
> VAR W = ROUND(MIN(1, MAX(0, [% del total])) * 100, 0)
> RETURN
> "data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='100' height='12'>" &
> "<rect width='100' height='12' fill='%23E0E0E0'/>" &
> "<rect width='" & W & "' height='12' fill='%231E88E5'/></svg>"
> ```

> [!success]- Si la imagen no aparece
> Revisa que la categoría de datos sea *URL de imagen* y que los `#` de colores estén como `%23`.

## Notas personales

_Qué me costó, errores que cometí, dudas._
