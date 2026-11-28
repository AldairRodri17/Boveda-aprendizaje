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
# Qué es la inteligencia de negocios (BI)

Parte de: [[17-Power-BI/01 Fundamentos/_Índice Fundamentos|Fundamentos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **inteligencia de negocios** (BI, *business intelligence*) es el conjunto de procesos, modelos de datos y herramientas que convierten los datos que genera una organización en información confiable para **tomar decisiones**. No es una herramienta: es una disciplina que va desde reunir datos de varios sistemas hasta poner frente a la persona correcta una cifra que pueda creer y sobre la que pueda actuar. Power BI es una de las herramientas para hacerlo.

Una buena solución de BI contesta tres tipos de preguntas: **qué pasó** (ventas del trimestre), **por qué pasó** (qué región o producto explica la caída) y **qué debería vigilar** (indicadores con meta y alerta). Las preguntas de "qué va a pasar" ya entran en analítica avanzada o ciencia de datos, que se apoya en la misma base.

### Cómo funciona
El flujo clásico separa el mundo que **opera** el negocio del mundo que lo **analiza**:

```mermaid
flowchart LR
  A[Sistemas OLTP<br/>ERP, punto de venta, CRM, API] --> B[Extracción y limpieza<br/>ETL / ELT, Power Query]
  B --> C[Modelo analítico<br/>estrella, almacén de datos]
  C --> D[Medidas y KPIs<br/>DAX]
  D --> E[Informes y paneles]
  E --> F[Decisión y acción]
```

**OLTP vs OLAP.** Un sistema transaccional (OLTP), como la base de Itzamna.Dionisio, está diseñado para registrar operaciones rápido y sin duplicar datos: muchas escrituras pequeñas, tablas muy normalizadas, consultas que tocan pocas filas (`INSERT` de un pedido, `UPDATE` de un estatus). Un sistema analítico (OLAP/BI) está diseñado para **leer muchísimas filas y agregarlas**: pocas escrituras (cargas por lotes), tablas desnormalizadas en forma de estrella, historia acumulada y consultas del tipo "suma de ventas por mes y categoría de los últimos tres años". Si lanzas esas consultas pesadas directo contra el OLTP, compites con las operaciones del negocio y obtienes un modelo difícil de entender.

**KPI.** Un indicador clave de desempeño es una medida ligada a un objetivo del negocio. Un número suelto ("vendimos 1.2 millones") no dice si es bueno o malo. Para que sea KPI necesita **contexto**: una **meta** (1.5 millones), un **umbral** que marque estado (verde si ≥ 100 % de la meta, amarillo entre 90 y 100 %, rojo debajo), un **periodo** definido y, de preferencia, una **tendencia** contra el periodo anterior. Además, alguien debe ser responsable de actuar cuando cambia de color.

### Ejemplo
Una aseguradora mexicana tiene su sistema de siniestros en SQL Server. Las tablas operativas (`Siniestro`, `Poliza`, `Ajustador`, `Pago`) sirven para capturar reclamaciones. El equipo de BI:

1. Extrae diario los siniestros cerrados y sus pagos.
2. Arma un modelo estrella: hechos `FactPagoSiniestro` y dimensiones `DimFecha`, `DimRamo`, `DimEstado`, `DimAjustador`.
3. Define KPIs: **siniestralidad** (pagos ÷ prima devengada) con meta de 65 %, y **días promedio de cierre** con meta de 30.
4. Publica un informe donde el director ve la siniestralidad por ramo, en rojo los que superan la meta, y puede bajar a estado y ajustador.

La decisión ("revisar las tarifas de autos en Jalisco") es el producto final, no la gráfica.

### Cuándo usarlo y cuándo no
- **Úsalo** cuando varias personas necesitan las mismas cifras con la misma definición, cuando hay que cruzar datos de varios sistemas o cuando la historia importa.
- **No hace falta** para una consulta puntual que resuelves con un `SELECT` o una tabla dinámica, ni para pantallas operativas en tiempo real dentro de la aplicación (eso es parte del producto, no de BI).
- Antes de construir, define con el usuario qué decisión quiere tomar; sin esa pregunta, el informe se vuelve un catálogo de gráficas.

### Errores comunes
- **"BI es hacer gráficas bonitas".** La gráfica es la última milla. Si los datos no están limpios, el modelo no es correcto o la medida está mal definida, una visual elegante solo comunica un error con más convicción. El 80 % del trabajo está en datos, modelo y definiciones.
- **Dos definiciones de la misma métrica.** Ventas con IVA en un informe y sin IVA en otro. Se evita con un modelo semántico compartido y un glosario.
- **Consultar directo el OLTP en horario pico.** Afecta la operación; usa réplicas, vistas o cargas programadas.
- **KPIs sin dueño ni meta.** Se convierten en números que nadie mira.

### Para profundizar
- [¿Qué es Power BI? (Microsoft Learn)](https://learn.microsoft.com/es-mx/power-bi/fundamentals/power-bi-overview)
- [Comprender el esquema de estrella y su importancia para Power BI](https://learn.microsoft.com/es-mx/power-bi/guidance/star-schema)

Siguiente paso: [[Ecosistema de Power BI (Desktop, Service, Mobile, Report Server y Embedded)]] y [[Flujo de trabajo (obtener, transformar, modelar, visualizar y publicar)]].

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es la inteligencia de negocios?
> El conjunto de procesos y herramientas para convertir datos operativos en información que apoye decisiones: extraer, limpiar, modelar, analizar y presentar (informes, tableros, indicadores).

> [!question]- ¿En qué se diferencia un sistema transaccional (OLTP) de uno analítico (OLAP/BI)?
> OLTP registra operaciones una a una (altas, cambios), muy normalizado y optimizado para escritura. Lo analítico lee grandes volúmenes, agrega y compara en el tiempo; suele usar modelos desnormalizados (estrella) optimizados para lectura.

> [!question]- ¿Qué es un KPI y qué le falta a un número suelto para serlo?
> Un indicador clave ligado a un objetivo. Un número suelto ("ventas = 1.2 M") no dice si va bien o mal; el KPI necesita meta o referencia (periodo anterior, presupuesto) y un dueño que actúe.

> [!question]- Error común: "BI es hacer gráficas bonitas". ¿Por qué no?
> La mayor parte del trabajo está en obtener datos confiables, definir bien las métricas y el modelo. Una gráfica con datos mal definidos lleva a malas decisiones aunque se vea bien.

