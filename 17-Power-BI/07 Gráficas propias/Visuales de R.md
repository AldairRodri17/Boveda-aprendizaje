---
proyecto: Conocimiento
tipo: concepto
nivel: avanzado
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
---
# Visuales de R

Parte de: [[17-Power-BI/07 Gráficas propias/_Índice Gráficas propias|Gráficas propias]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **visual de R** funciona igual que uno de Python, pero con el lenguaje **R**: Power BI pasa los campos del visual a un *data frame* llamado `dataset`, ejecuta tu script y muestra como imagen el gráfico resultante (con `ggplot2`, `lattice` o gráficos base). R es fuerte en estadística y tiene paquetes maduros para series de tiempo, modelos y visualización estadística.

### Cómo funciona
**Qué necesitas en Desktop**
1. Instalar **R** en tu equipo (desde CRAN; también sirve una distribución compatible) y los paquetes que uses (`install.packages("ggplot2")`).
2. En **Archivo → Opciones → Scripts de R**, indicar la carpeta de instalación de R (Power BI la detecta si está en la ruta estándar). Opcionalmente, un IDE externo como RStudio para editar.
3. Agregar el visual **R** al lienzo y habilitar los visuales de script.
4. Arrastrar campos a **Valores** y escribir el script en el editor inferior.

```r
# Preámbulo que genera Power BI (comentado):
# dataset <- data.frame(Mes, Ventas)
# dataset <- unique(dataset)

library(ggplot2)

ggplot(dataset, aes(x = Mes, y = Ventas)) +
  geom_point(color = "#1E3A5F") +
  geom_smooth(method = "loess", se = TRUE, color = "#F28C28") +
  labs(title = "Ventas mensuales con tendencia suavizada", x = NULL, y = "Ventas") +
  theme_minimal()
```

Igual que en Python, las filas idénticas se **combinan** (por el `unique`), así que agrega un identificador si necesitas todas.

**Limitaciones** (comparten la lógica de los visuales de Python; detalle exacto de R pendiente de confirmar en la página específica):
- Límite de filas (150,000) y tiempo máximo de ejecución; resolución fija de imagen.
- **Imagen estática**: no hay tooltips ni filtro cruzado desde el visual.
- En el Service se ejecutan en un entorno de Microsoft con una **lista cerrada de paquetes de R compatibles** y versión de R definida por Microsoft; requieren licencia Pro/PPU (o capacidad para consumidores gratuitos) y no están disponibles en todos los escenarios de inserción (no en "app owns data").
- Riesgo de seguridad: el script se ejecuta en tu equipo al abrir el archivo en Desktop; abre solo informes de fuentes confiables.

```mermaid
flowchart LR
  C[Campos] --> D[dataset data.frame único]
  D --> R[Script R con ggplot2]
  R --> I[Imagen estática]
```

### Ejemplo
En el equipo de análisis de Itzamna quieren un gráfico de caja (*boxplot*) de tiempo de entrega por paquetería. Con `ggplot(dataset, aes(Paqueteria, Horas)) + geom_boxplot()` y `Envio[Guia]` en los campos (para no perder duplicados), lo tienen en minutos. Como no se necesita interactividad y lo consumen usuarios internos con Pro, es aceptable.

### Cuándo usarlo y cuándo no
- **Úsalo** si tu equipo ya sabe R y necesitas un gráfico estadístico puntual.
- **No** para informes masivos o externos, ni si necesitas interacción: prefiere nativos o [[Deneb (Vega y Vega-Lite)]].
- Si el equipo trabaja en Python, usa [[Visuales de Python]]; mantener dos lenguajes de script complica.

### Errores comunes
1. **No tener R instalado** en el equipo de quien abre el .pbix en Desktop: el visual muestra error.
2. **Paquete no compatible en el Service**: funciona local y falla publicado.
3. **Filas combinadas** por duplicados.
4. **Gráficos con mucho detalle** que se ven pixelados por la resolución fija.
5. **Confundir el visual con un modelo**: el script corre en cada interacción; cálculos pesados lo hacen lento.

### Para profundizar
- [Crear visuales de R en Power BI Desktop (Microsoft Learn)](https://learn.microsoft.com/en-us/power-bi/create-reports/desktop-r-visuals)
- [Paquetes de R compatibles en el Service](https://learn.microsoft.com/en-us/power-bi/connect-data/service-r-packages-support)
- [ggplot2 (tidyverse)](https://ggplot2.tidyverse.org/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué necesitas para usar un visual de R en Desktop?
> R instalado localmente y configurado en *Opciones → Scripts de R*; los campos llegan como un data frame `dataset`.

> [!question]- ¿Qué limitaciones tiene?
> Es una imagen estática (sin interacción interna), tiene límites de filas y tiempo de ejecución, y en el Service solo admite paquetes soportados (verificar en la documentación).

