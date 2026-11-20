---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: SQL Server
tags: [sql-server]
---
# Primera forma normal (1FN)

Parte de: [[Normalización]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **primera forma normal (1FN)** es el requisito mínimo de una tabla relacional: cada columna guarda **un solo valor atómico** del dominio de esa columna, no hay **grupos repetidos** de columnas, y cada fila se puede identificar por una **clave**. Es el primer paso de la [[Normalización]]; las demás formas normales la dan por hecho.

### Cómo funciona
Una tabla está en 1FN cuando:

1. **Cada celda tiene un valor**, no una lista ni una estructura que haya que partir para usarla. `Telefonos = '33-1234-5678, 55-8765-4321'` no cumple: para buscar quién tiene un teléfono hay que usar `LIKE '%...%'` o `STRING_SPLIT`.
2. **No hay columnas repetidas** para el mismo concepto: `Producto1, Cantidad1, Producto2, Cantidad2, Producto3...` es un grupo repetido "aplanado" en columnas.
3. **Hay una clave** que identifica cada fila; sin ella, puede haber filas duplicadas indistinguibles.
4. El orden de filas y columnas no tiene significado (consecuencia del [[Modelo relacional]]).

**¿Qué es "atómico"?** Depende de **cómo se usa** el dato. Una fecha es atómica aunque tenga año, mes y día, porque la base la maneja como un valor con funciones propias. Un nombre completo puede ser atómico si nunca necesitas buscar por apellido; si sí lo necesitas, conviene separarlo. La pregunta útil es: *¿necesito filtrar, ordenar, validar o relacionar por una parte de este valor?* Si la respuesta es sí, esa parte debería ser columna (o fila en otra tabla).

**¿Y un JSON en `nvarchar(max)`?** No viola automáticamente la 1FN si la base lo trata como **un valor opaco**: una configuración, la respuesta cruda de un proveedor o un documento que siempre se lee y escribe completo. Sí la viola en espíritu si empiezas a filtrar, unir o validar por propiedades internas del JSON, porque estás guardando estructura relacional dentro de un texto sin restricciones ni FK. SQL Server ofrece `OPENJSON` y `JSON_VALUE` para esos casos, pero si la consulta por campos internos es frecuente, esos campos merecen columnas propias. Ver [[JSON en SQL Server (OPENJSON y FOR JSON)]].

### Ejemplo
Diseño que viola 1FN y su corrección:

```sql
-- MAL: grupo repetido en columnas y lista en una celda
CREATE TABLE dbo.PedidoPlano (
    PedidoId   int PRIMARY KEY,
    Producto1  nvarchar(100), Cantidad1 int,
    Producto2  nvarchar(100), Cantidad2 int,
    Producto3  nvarchar(100), Cantidad3 int,
    Telefonos  nvarchar(200)          -- '33-1234-5678, 55-8765-4321'
);

-- BIEN: una fila por renglón y una fila por teléfono
CREATE TABLE ventas.PedidoDetalle (
    PedidoId   int NOT NULL,
    ProductoId int NOT NULL,
    Cantidad   int NOT NULL,
    CONSTRAINT PK_PedidoDetalle PRIMARY KEY (PedidoId, ProductoId)
);
CREATE TABLE ventas.ClienteTelefonos (
    ClienteId int NOT NULL REFERENCES ventas.Clientes (ClienteId),
    Telefono  varchar(15) NOT NULL,
    Tipo      varchar(10) NOT NULL CHECK (Tipo IN ('Móvil', 'Casa', 'Trabajo')),
    CONSTRAINT PK_ClienteTelefonos PRIMARY KEY (ClienteId, Telefono)
);

-- Ahora buscar por teléfono es una igualdad indexable
SELECT c.Nombre
FROM   ventas.ClienteTelefonos t
JOIN   ventas.Clientes c ON c.ClienteId = t.ClienteId
WHERE  t.Telefono = '3312345678';
```

Problemas concretos de `Producto1..Producto3`: un pedido con cuatro productos no cabe (hay que hacer `ALTER TABLE`); sumar unidades exige `Cantidad1 + Cantidad2 + Cantidad3` con manejo de `NULL`; buscar "pedidos con teclado" requiere `OR` en tres columnas; y no puedes poner una FK que valide los productos de forma limpia ni evitar que el mismo producto aparezca dos veces.

Si necesitas **migrar** una lista separada por comas existente, `STRING_SPLIT` ayuda a convertirla en filas una sola vez:

```sql
INSERT INTO ventas.ClienteTelefonos (ClienteId, Telefono, Tipo)
SELECT c.ClienteId, REPLACE(TRIM(s.value), '-', ''), 'Móvil'
FROM   dbo.ClientesViejos c
CROSS  APPLY STRING_SPLIT(c.Telefonos, ',') s;
```

### Cuándo usarlo y cuándo no
- **Cumple 1FN** en todo dato que vayas a consultar por partes, validar o relacionar.
- **Un JSON o texto opaco** es aceptable para datos que la base no interpreta (configuración por usuario, bitácoras crudas de integraciones).
- **Un arreglo pequeño y fijo** que siempre se lee completo (por ejemplo, coordenadas X/Y) puede quedarse en columnas con nombres significativos (`Latitud`, `Longitud`), porque no es un grupo repetido sino atributos distintos.

### Errores comunes
1. **Listas separadas por comas** en una columna: búsquedas lentas, sin integridad y con errores de formato.
2. **Columnas numeradas** (`Telefono1`, `Telefono2`): límite artificial y consultas con `OR`.
3. **Tablas sin clave** que admiten filas duplicadas.
4. **JSON como base de datos dentro de la base**: filtrar y unir por propiedades internas sin índices ni restricciones.
5. **Separar de más**: partir una fecha en tres columnas `Anio`, `Mes`, `Dia` pierde validación y funciones de fecha.

### Para profundizar
- [Descripción de la normalización de bases de datos](https://learn.microsoft.com/office/troubleshoot/access/database-normalization-description)
- [STRING_SPLIT (Transact-SQL)](https://learn.microsoft.com/sql/t-sql/functions/string-split-transact-sql)
- [Datos JSON en SQL Server](https://learn.microsoft.com/sql/relational-databases/json/json-data-sql-server)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué exige la 1FN?
> Que cada columna tenga valores **atómicos** (un solo valor) y que no haya grupos repetidos (`Telefono1`, `Telefono2`, `Telefono3`). Cada fila se identifica por una clave.

> [!question]- ¿Guardar un documento JSON en `nvarchar(max)` viola la 1FN?
> Formalmente el JSON no es atómico, pero si la base lo trata como un valor opaco (un snapshot que se lee entero) es una decisión válida. Si necesitas filtrar o unir por sus campos con frecuencia, conviene modelarlo en columnas o tablas.

> [!question]- Error común: columnas `Producto1, Cantidad1, Producto2, Cantidad2`. ¿Qué problemas trae?
> Límite fijo de elementos, consultas con `OR` por cada columna, `NULL` por todos lados y agregaciones imposibles. Se resuelve con una tabla hija (una fila por elemento).

