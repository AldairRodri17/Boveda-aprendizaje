---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
---
# SQLBI y otros recursos para aprender DAX

Parte de: [[17-Power-BI/05 Ayudas para crear DAX más rápido/_Índice Ayudas para crear DAX más rápido|Ayudas para crear DAX más rápido]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**SQLBI** es la empresa de Marco Russo y Alberto Ferrari, autores de referencia en DAX y modelado tabular. Publican gratis artículos, videos y herramientas, y venden cursos y libros avanzados. Junto con la documentación oficial de Microsoft, forman el núcleo de fuentes confiables para aprender DAX; el resto (blogs, foros, videos) conviene contrastarlo con ellas.

### Cómo funciona
**Recursos de SQLBI y su ecosistema**
- **Artículos** en sqlbi.com sobre contexto de filtro, transición de contexto, patrones, rendimiento. Muy buenos para profundizar en una duda puntual.
- **DAX Guide** (dax.guide): referencia de cada función con sintaxis, ejemplos, compatibilidad por producto y notas sobre comportamiento. Más detallada que la referencia oficial en casos raros.
- **DAX Patterns** (daxpatterns.com): soluciones listas y explicadas para problemas comunes (acumulados, ABC, semiaditivos, clientes nuevos y recurrentes, presupuestos).
- **DAX Formatter** (gratuito) y herramientas como **Bravo** (gratuita y de código abierto); además de VertiPaq Analyzer, que se usa integrado en DAX Studio ([[DAX Studio]], [[DAX Formatter y guías de estilo]]).
- **Curso gratuito "Introducing DAX"** (video) para empezar; cursos de pago (como *Mastering DAX*) y el libro *The Definitive Guide to DAX* para nivel avanzado (precios: Pendiente de confirmar en su tienda).
- Videos en su canal de YouTube y boletín.

**Recursos oficiales gratuitos de Microsoft**
- **Referencia de DAX** en Microsoft Learn: todas las funciones, sintaxis, operadores y guías de buenas prácticas (DIVIDE vs `/`, variables, evitar FILTER como argumento).
- **Ruta de aprendizaje de DAX** en Microsoft Learn (módulos con ejercicios) y la documentación de Power BI (medidas, cálculos visuales, vista de consultas DAX).
- **Blog de actualizaciones de Power BI** (resúmenes mensuales) para enterarte de funciones nuevas.

**Comunidad**: foros de la comunidad de Fabric/Power BI para preguntas puntuales; úsalos con criterio y verifica las respuestas.

### Ejemplo
Plan de estudio de 6 semanas con el modelo de ejemplo de esta sección (Ventas, Productos, Calendario; total 1 450):

| Semana | Tema | Fuente principal | Práctica |
|---|---|---|---|
| 1 | Medidas, columnas, sintaxis | Microsoft Learn (referencia y ruta) | `Ventas $`, `Costo $`, `% Margen` |
| 2 | Contexto de fila y de filtro | Artículos SQLBI | Explicar por qué Bebidas = 500 |
| 3 | CALCULATE y transición | Curso "Introducing DAX" + DAX Guide | `% del total`, columna "Vendió más de 400" |
| 4 | Inteligencia de tiempo | Microsoft Learn + DAX Patterns | YTD (700), AA (550), Var % (−9.09 %) |
| 5 | Tablas virtuales y patrones | DAX Patterns | Pareto y ABC (A Equipo, B Bebidas, C Alimentos) |
| 6 | Rendimiento | DAX Studio + artículos SQLBI | Comparar dos versiones de una medida |

Regla: para cada concepto, lee la referencia oficial, luego el artículo de SQLBI, y valida con tu propio ejemplo en la [[Vista de consultas DAX (DAX query view)]].

### Cuándo usarlo y cuándo no
- **Microsoft Learn** para la definición oficial, comportamiento documentado y novedades.
- **DAX Guide** cuando una función se comporta raro o necesitas saber en qué productos existe.
- **DAX Patterns** cuando el problema "ya lo resolvió alguien": no reinventes el ABC.
- **Artículos de SQLBI** para entender el porqué (contextos, transición, motor).
- **Cuidado** con tutoriales que copian fórmulas sin explicar el contexto, o que son de versiones viejas de Power BI.

### Errores comunes
- **Aprender solo con videos cortos** y copiar fórmulas sin entender contexto de filtro.
- **Saltar directo a patrones avanzados** sin dominar CALCULATE.
- **No practicar con datos pequeños** donde puedes verificar el resultado a mano.
- **Fiarse de respuestas de foros o IA** sin contrastarlas con la referencia.

### Para profundizar
- [SQLBI](https://www.sqlbi.com/)
- [DAX Guide](https://dax.guide/)
- [DAX Patterns](https://www.daxpatterns.com/)
- [Referencia de DAX (Microsoft Learn)](https://learn.microsoft.com/en-us/dax/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es SQLBI y qué recursos ofrece?
> El sitio de Marco Russo y Alberto Ferrari: artículos, patrones (daxpatterns.com), dax.guide (referencia de funciones), videos y cursos (algunos de pago).

> [!question]- Menciona dos recursos oficiales gratuitos.
> La documentación de DAX en Microsoft Learn y las rutas de aprendizaje de Power BI (incluidas las del examen PL-300) en Microsoft Learn.

