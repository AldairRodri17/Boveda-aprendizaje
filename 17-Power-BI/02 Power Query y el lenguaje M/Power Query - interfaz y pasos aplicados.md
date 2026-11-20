---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
---
# Power Query - interfaz y pasos aplicados

Parte de: [[17-Power-BI/02 Power Query y el lenguaje M/_Índice Power Query y el lenguaje M|Power Query y el lenguaje M]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El **Editor de Power Query** es la ventana donde se transforman los datos antes de cargarlos al modelo. Se abre desde Desktop con *Inicio > Transformar datos*. Todo lo que haces con clics queda registrado como una lista de **pasos aplicados**, y cada paso es una línea de código **M**. Power Query es el mismo motor en Power BI, Excel, Dataflows y Fabric, así que lo que aprendas aquí te sirve en todos.

### Cómo funciona
Partes de la ventana:

- **Consultas** (izquierda): cada consulta produce una tabla (o una lista, valor o función). Puedes agruparlas en carpetas: `Origen`, `Dimensiones`, `Hechos`, `Funciones`.
- **Vista previa** (centro): primeras filas del resultado del paso seleccionado.
- **Barra de fórmulas**: el código M del paso actual (actívala en *Vista > Barra de fórmulas*).
- **Configuración de consulta** (derecha): nombre y **pasos aplicados**.
- **Editor avanzado**: el script M completo de la consulta, con su `let ... in`.

```mermaid
flowchart LR
  O[Origen] --> N[Navegación] --> E[Encabezados promovidos] --> T[Tipo cambiado] --> F[Filas filtradas] --> R[Resultado]
```

**Los pasos aplicados son una cadena**: cada paso recibe como entrada el resultado del paso anterior (por su nombre). En M se ve así: `#"Filas filtradas" = Table.SelectRows(#"Tipo cambiado", each ...)`. Si **eliminas un paso intermedio**, el siguiente sigue apuntando a un nombre que ya no existe y aparece `Expression.Error: No se reconoce el nombre...`, o bien el paso siguiente falla porque esperaba una columna que el paso eliminado creaba. Para borrar sin romper, usa *Eliminar hasta el final* o corrige la referencia en la barra de fórmulas. Al hacer clic en un paso anterior ves los datos en ese punto; si agregas un paso ahí, Power Query te avisa que se insertará en medio.

**Herramientas de perfilado** (pestaña *Vista*):
- **Calidad de columna**: porcentaje de valores válidos, con error y vacíos en cada columna. Detecta de inmediato una columna con 3 % de errores de conversión.
- **Distribución de columnas**: cuántos valores distintos y únicos tiene; útil para saber si una columna sirve como clave (distintos = únicos = filas).
- **Perfil de columna**: estadísticas (mín., máx., promedio, desviación) e histograma de la columna seleccionada.

Ojo: por defecto se calculan sobre las **primeras 1 000 filas**. Cambia a *Generación de perfiles en todo el conjunto de datos* en la barra de estado cuando quieras certeza.

**Habilitar carga**: en el menú contextual de la consulta. Si lo desactivas, la consulta **no se carga al modelo** (no aparece en la vista Modelo ni ocupa memoria) pero sigue existiendo para que otras la referencien. Se usa para consultas intermedias: el origen crudo, tablas de staging, listas o funciones. Se ven en cursiva.

### Ejemplo
Organización recomendada para un modelo de ventas desde SQL Server:

```m
// Consulta "src_Dionisio" (carga deshabilitada): solo la conexión
let
    Origen = Sql.Database(pServidor, pBase)
in
    Origen

// Consulta "Ventas" (se carga): referencia a la anterior
let
    Origen = src_Dionisio,
    Ventas = Origen{[Schema = "bi", Item = "vw_Ventas"]}[Data],
    #"Columnas quitadas" = Table.RemoveColumns(Ventas, {"UsuarioAlta", "FechaAlta"}),
    #"Filas filtradas" = Table.SelectRows(#"Columnas quitadas", each [Fecha] >= #date(2023, 1, 1))
in
    #"Filas filtradas"
```

Renombra los pasos con nombres descriptivos (clic derecho > *Cambiar nombre*) y agrega descripciones (*Propiedades*): el editor muestra el texto como información sobre herramientas.

### Cuándo usarlo y cuándo no
- **Usa la interfaz** para explorar y para el 80 % de las transformaciones; genera M correcto y plegable.
- **Usa el editor avanzado** para lógica que la interfaz no ofrece, para copiar consultas entre archivos o para limpiar pasos redundantes.
- **Referencia vs duplicar**: *Referencia* crea una consulta que parte del resultado de otra (cambios arriba se propagan); *Duplicar* copia todos los pasos (quedan independientes).

### Errores comunes
- **Decenas de pasos `Tipo cambiado`** repetidos: consolida en uno al final.
- **Eliminar un paso a mitad** y romper los siguientes.
- **Confiar en el perfil de 1 000 filas** para decidir que no hay errores.
- **Cargar consultas intermedias** al modelo: duplican memoria y confunden a los autores de informes.
- **Nombres de pasos genéricos** (`Personalizado1`, `Paso3`) que nadie entiende seis meses después.

### Para profundizar
- [La interfaz de usuario de Power Query](https://learn.microsoft.com/es-mx/power-query/power-query-ui)
- [Herramientas de generación de perfiles de datos](https://learn.microsoft.com/es-mx/power-query/data-profiling-tools)
- [Pasos aplicados](https://learn.microsoft.com/es-mx/power-query/applied-steps)

Relacionado: [[Lenguaje M - let e in]], [[Power Query - limpieza de datos]].

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué son los *pasos aplicados*?
> La lista ordenada de transformaciones de la consulta. Cada paso es una expresión M que toma el resultado del anterior; puedes ver el código en el *Editor avanzado*.

> [!question]- ¿Qué pasa si eliminas un paso intermedio?
> Los pasos siguientes que hacen referencia a columnas o al nombre de ese paso pueden fallar ("no se encontró la columna"). Hay que revisar los posteriores.

> [!question]- ¿Para qué sirven *Calidad de columna*, *Distribución* y *Perfil*?
> Muestran porcentaje de válidos/errores/vacíos, valores distintos y únicos, y estadísticas. Ojo: por defecto se calculan sobre las primeras 1 000 filas; se puede cambiar a todo el conjunto en la barra de estado.

> [!question]- ¿Qué hace desactivar *Habilitar carga* en una consulta?
> La consulta sigue existiendo (para usarla en merges o como base) pero no se carga como tabla al modelo; útil para consultas intermedias.

## Práctica

- [[Lab 2 - Limpieza y combinación de datos en Power Query]]

