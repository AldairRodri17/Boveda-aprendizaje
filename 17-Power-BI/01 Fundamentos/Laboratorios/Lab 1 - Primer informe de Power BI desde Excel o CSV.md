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
# Lab 1 - Primer informe de Power BI desde Excel o CSV

Parte de: [[17-Power-BI/01 Fundamentos/_Índice Fundamentos|Fundamentos]]

**Cubre:** [[Instalar Power BI Desktop y primer informe]] · [[Flujo de trabajo (obtener, transformar, modelar, visualizar y publicar)]] · [[Conectores y orígenes de datos (Excel, CSV, SQL Server, Web y API)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Instalar Power BI Desktop, cargar dos archivos CSV (o un Excel con dos tablas), relacionarlos y construir una página con KPI, barras y tabla.

Datos (guárdalos en esta carpeta como `ventas.csv` y `productos.csv`, o como dos tablas de Excel):

```text
ventas.csv
IdVenta,Fecha,FechaEnvio,IdProducto,Cantidad,PrecioUnitario,CostoUnitario
1,2024-01-15,2024-01-18,P1,2,100,60
2,2024-02-10,2024-02-12,P2,1,250,150
3,2024-02-20,2024-02-25,P1,3,100,60
4,2025-01-05,2025-01-07,P3,5,40,25
5,2025-02-14,2025-02-20,P2,2,250,150

productos.csv
IdProducto,Producto,Categoria,Color
P1,Café,Bebidas,Negro
P2,Molino,Equipo,Rojo
P3,Galletas,Alimentos,Café
```

## Pasos

1. Instala Power BI Desktop desde Microsoft Store y abre un informe en blanco.
2. En *Archivo → Opciones → Carga de datos* desactiva *Fecha/hora automática* para este archivo.
3. *Obtener datos → Texto/CSV* y carga `ventas.csv`; revisa en *Transformar datos* que Fecha sea tipo fecha y los importes números.
4. Carga `productos.csv` del mismo modo.
5. En la vista Modelo verifica (o crea) la relación Productos[IdProducto] 1 → * Ventas[IdProducto].
6. Crea la medida `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` con formato moneda.
7. En la vista Informe agrega: una tarjeta con Ventas $, un gráfico de barras Categoria vs Ventas $ y una tabla Producto / Ventas $.
8. Agrega una segmentación por Categoria y prueba que filtra los otros visuales.
9. Guarda como `Lab1.pbix` en esta carpeta.

## Criterios de terminado

- [ ] La tarjeta muestra **1,450** sin filtros.
- [ ] La barra de Equipo es la más larga (750) y Alimentos la menor (200).
- [ ] Al elegir Bebidas en la segmentación la tarjeta muestra 500.
- [ ] La relación es 1:* con dirección de filtro simple de Productos a Ventas.
- [ ] El archivo se guardó y al reabrirlo actualiza sin errores.

## Solución

> [!success]- Resultados esperados
> Total 1 450; Equipo 750, Bebidas 500, Alimentos 200; tabla: Molino 750, Café 500, Galletas 200.

> [!success]- Si la relación no se creó sola
> Arrastra Productos[IdProducto] sobre Ventas[IdProducto] en la vista Modelo. Si dice que ambas son "varios", revisa que no haya IdProducto repetidos en productos.csv.

> [!success]- Si el total sale 0 o en texto
> Cantidad o PrecioUnitario quedaron como texto: en Power Query cámbialos a Número entero / Número decimal y *Cerrar y aplicar*.

## Notas personales

_Qué me costó, errores que cometí, dudas._
