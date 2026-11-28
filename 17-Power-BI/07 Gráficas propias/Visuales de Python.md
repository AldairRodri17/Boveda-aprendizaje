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
# Visuales de Python

Parte de: [[17-Power-BI/07 Gráficas propias/_Índice Gráficas propias|Gráficas propias]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **visual de Python** ejecuta un script de Python sobre los campos que arrastras al visual y muestra como **imagen** el gráfico que genere (normalmente con matplotlib o seaborn). Sirve para gráficos estadísticos que Power BI no trae: distribuciones con densidad, matrices de correlación, regresiones con intervalos, pronósticos.

### Cómo funciona
1. En Desktop instala Python (y paquetes como `pandas` y `matplotlib`) en tu equipo y configúralo en **Archivo → Opciones → Scripts de Python** (ruta del intérprete).
2. Agrega el visual **Py** y habilita los visuales de script si lo pide.
3. Arrastra campos al área **Valores**. Power BI crea automáticamente un **DataFrame de pandas llamado `dataset`** con esas columnas.
4. Escribe el script en el editor de la parte inferior; Power BI muestra el resultado de `plt.show()`.

El código generado se ve así (las primeras líneas las escribe Power BI como comentario):

```python
# El siguiente código para crear un dataframe y quitar filas duplicadas siempre se ejecuta
# y actúa como preámbulo del script:
# dataset = pandas.DataFrame(Monto, Dias)
# dataset = dataset.drop_duplicates()

import matplotlib.pyplot as plt

fig, ax = plt.subplots(figsize=(8, 4))
ax.hist(dataset["Dias"], bins=20, color="#1E3A5F")
ax.set_title("Distribución de días de pago")
ax.set_xlabel("Días")
ax.set_ylabel("Facturas")
plt.tight_layout()
plt.show()
```

**Filas duplicadas**: el visual agrupa los campos (por defecto *No resumir*) y las filas idénticas aparecen **una sola vez** en `dataset`. Si dos facturas tienen el mismo monto y los mismos días, verás una. Para conservar todas, agrega un campo que haga única cada fila (un índice o el folio).

**Limitaciones** (Microsoft Learn, consultado nov 2026):
- Máximo **150,000 filas** (se toman las primeras) y 250 MB de datos de entrada.
- El cálculo falla si tarda más de **5 minutos**.
- La imagen se muestra a 72 DPI y **no es interactiva**: responde a filtros y resaltado de otros visuales, pero no puede filtrar a otros.
- Solo se renderiza el dispositivo de gráficos predeterminado.
- **En el Service**: crear y ver requiere **Pro o PPU** (los usuarios gratuitos solo los consumen en workspaces Premium/Fabric), y solo funciona en regiones de Fabric con disponibilidad de Spark. El Service usa su propio entorno con una **lista de paquetes compatibles**; un paquete que tengas local puede no existir ahí. No funcionan en "insertar para sus clientes" (app owns data).
- Power BI muestra una advertencia de seguridad la primera vez: un script puede tener riesgos.

```mermaid
flowchart LR
  C[Campos] --> D[dataset: DataFrame sin duplicados]
  D --> S[Script Python]
  S --> I[Imagen PNG estática]
```

### Ejemplo
Para la cartera de Pochteca.Nomisma quieres ver la distribución de días de pago con la mediana marcada: arrastras `Factura[Folio]` (para no perder duplicados) y `Factura[DiasPago]`, y el script dibuja histograma y una línea vertical con `ax.axvline(dataset["DiasPago"].median())`.

### Cuándo usarlo y cuándo no
- **Úsalo** para análisis estadístico puntual y gráficos que necesitan una librería de Python.
- **No** para gráficos de negocio comunes ni cuando necesitas interactividad: usa nativos o [[Deneb (Vega y Vega-Lite)]].
- **No** en informes para clientes externos embebidos ni si el tenant está fuera de regiones compatibles.
- Si el cálculo estadístico es lo importante, considera calcularlo antes (en Fabric, notebooks o en el ETL) y graficarlo con nativos.

### Errores comunes
1. **Perder filas por duplicados** sin darse cuenta. Agrega un identificador.
2. **Usar un paquete que no está en el Service**: funciona en Desktop y falla al publicar.
3. **Renombrar columnas del visual** y esperar el nuevo nombre en el script: se usa el nombre original.
4. **Mandar millones de filas**: se corta en 150,000 y el gráfico engaña.
5. **Olvidar `plt.show()`** o usar otro backend: no aparece nada.

### Para profundizar
- [Crear visuales de Python en Power BI Desktop (Microsoft Learn)](https://learn.microsoft.com/en-us/power-bi/connect-data/desktop-python-visuals)
- [Paquetes de Python compatibles en el Service](https://learn.microsoft.com/en-us/power-bi/connect-data/service-python-packages-support)
- [Documentación de matplotlib](https://matplotlib.org/stable/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo se ve el código de un visual de Python?
> Recibe `dataset` (pandas DataFrame) con los campos arrastrados y se dibuja con matplotlib/seaborn:
> ```python
> import matplotlib.pyplot as plt
> dataset.plot(kind='bar', x='Categoria', y='Ventas')
> plt.show()
> ```

> [!question]- ¿Qué pasa con las filas duplicadas en `dataset`?
> Power BI elimina filas duplicadas de los campos; si necesitas todas, agrega una columna única (índice).

> [!question]- Limitación en el Service.
> Solo ciertos paquetes y versiones están soportados en la nube; además hay límites de tiempo y tamaño (verificar en la documentación).

