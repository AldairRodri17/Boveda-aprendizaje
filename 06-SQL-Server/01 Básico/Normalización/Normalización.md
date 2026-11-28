---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: SQL Server
tags: [sql-server]
contenido: completo
---
# Normalización

Parte de: [[06-SQL-Server/01 Básico/_Índice Básico|Básico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **normalización** es el proceso de organizar las tablas para que **cada hecho se guarde en un solo lugar**. Se hace descomponiendo una tabla grande en varias más pequeñas relacionadas por claves, siguiendo una serie de reglas llamadas **formas normales**. El objetivo es eliminar redundancia y, con ella, las **anomalías**: datos que se contradicen porque se actualizaron en un lugar y no en otro.

### Cómo funciona
**Las anomalías que resuelve**, con una tabla plana `VentasPlano(PedidoId, Fecha, ClienteNombre, ClienteCiudad, ProductoNombre, Categoria, Cantidad, PrecioUnitario)`:

- **De actualización**: si Ana se muda, hay que cambiar su ciudad en todas sus filas; si se olvida una, la base dice dos cosas distintas.
- **De inserción**: no puedes registrar un producto nuevo hasta que alguien lo compre, porque no hay fila de venta donde ponerlo.
- **De borrado**: si borras el único pedido de un cliente, pierdes también los datos del cliente.

**La idea central es la dependencia funcional**: `A → B` significa "conociendo A, conozco B" (conociendo `ClienteId`, conozco `ClienteCiudad`). Normalizar es asegurar que cada atributo dependa **de la clave, de toda la clave y nada más que de la clave**.

**Las formas normales**, cada una exige la anterior:

| Forma | Regla en una frase | Nota |
|---|---|---|
| 1FN | Valores atómicos, sin grupos repetidos | [[Primera forma normal (1FN)]] |
| 2FN | Nada depende de solo una parte de una clave compuesta | [[Segunda forma normal (2FN)]] |
| 3FN | Nada depende de otro atributo que no sea clave (sin dependencias transitivas) | [[Tercera forma normal (3FN)]] |
| FNBC | Todo determinante es clave candidata | [[Forma normal de Boyce-Codd (FNBC)]] |

Existen 4FN y 5FN (dependencias multivaluadas y de unión), pero en la práctica rara vez se aplican de forma explícita.

**Hasta dónde llegar**: un diseño OLTP (el de una API como Itzamna.Dionisio) normalmente apunta a **3FN**, que casi siempre coincide con FNBC. Después, si una consulta crítica lo justifica, se aplica una [[Desnormalización deliberada]], controlada y documentada. Los modelos analíticos (Power BI, data warehouse) suelen desnormalizar a propósito en esquemas de estrella.

```mermaid
flowchart LR
  A[Tabla plana] -->|valores atómicos| B[1FN]
  B -->|quitar dependencias parciales| C[2FN]
  C -->|quitar dependencias transitivas| D[3FN]
  D -->|todo determinante es clave| E[FNBC]
  E -.->|si el rendimiento lo exige| F[Desnormalización deliberada]
```

### Ejemplo
Normalizar `VentasPlano` paso a paso:

1. **1FN**: ya tiene valores atómicos; definimos la clave `(PedidoId, ProductoNombre)`.
2. **2FN**: `Fecha`, `ClienteNombre` y `ClienteCiudad` dependen solo de `PedidoId` (parte de la clave); `Categoria` depende solo de `ProductoNombre`. Se separan en `Pedidos` y `Productos`.
3. **3FN**: en `Pedidos`, `ClienteCiudad` depende de `ClienteNombre`, no del pedido (dependencia transitiva `PedidoId → Cliente → Ciudad`). Se separa `Clientes`.

```sql
CREATE TABLE ventas.Clientes  (ClienteId int PRIMARY KEY, Nombre nvarchar(100) NOT NULL, Ciudad nvarchar(60) NULL);
CREATE TABLE ventas.Productos (ProductoId int PRIMARY KEY, Nombre nvarchar(100) NOT NULL UNIQUE,
                               Categoria nvarchar(50) NOT NULL, Precio decimal(10,2) NOT NULL);
CREATE TABLE ventas.Pedidos   (PedidoId int PRIMARY KEY, Fecha date NOT NULL,
                               ClienteId int NOT NULL REFERENCES ventas.Clientes (ClienteId));
CREATE TABLE ventas.PedidoDetalle (
    PedidoId int NOT NULL REFERENCES ventas.Pedidos (PedidoId),
    ProductoId int NOT NULL REFERENCES ventas.Productos (ProductoId),
    Cantidad int NOT NULL,
    PrecioUnitario decimal(10,2) NOT NULL,   -- precio pactado en esa venta
    PRIMARY KEY (PedidoId, ProductoId));
```

**¿`PrecioUnitario` en `PedidoDetalle` rompe la normalización?** No: no es una copia de `Productos.Precio`, es **otro hecho**. `Productos.Precio` es el precio de lista *hoy*; `PrecioUnitario` es lo que se cobró *en esa venta* (con descuento o antes de un cambio de precio). Depende de la clave completa `(PedidoId, ProductoId)`. Si lo quitaras, cambiar un precio de lista alteraría el importe de pedidos viejos. Lo mismo pasa con la dirección de entrega de un pedido frente a la dirección actual del cliente.

Después de normalizar, el JOIN rearma la vista plana cuando se necesita (ver [[JOINs]]), y una vista puede ofrecerla a los reportes (ver [[Vistas]]).

### Cuándo usarlo y cuándo no
- **Normaliza siempre al diseñar datos transaccionales**: escrituras frecuentes, reglas de integridad, muchos usuarios.
- **Desnormaliza con criterio** en reportes, cachés, tablas de lectura o datos históricos inmutables, y documenta cómo se mantiene la copia consistente.
- **Inmutables y snapshots**: guardar una "foto" de los datos en el momento de un evento no es redundancia, es historia (ver [[Snapshot inmutable versionado]]).

### Errores comunes
1. **Normalizar de más**: separar en tablas datos que no tienen dependencias propias (una tabla `Nombres` y otra `Apellidos`) solo agrega JOIN.
2. **Confundir historia con redundancia** y eliminar `PrecioUnitario` o direcciones de envío.
3. **Listas en una columna** (`Telefonos = '33-..., 55-...'`): viola 1FN.
4. **Desnormalizar "por si acaso"** sin medir; luego nadie recuerda que hay que mantener dos copias.
5. **Ignorar las claves candidatas** al normalizar: sin `UNIQUE` en el nombre del producto, la tabla `Productos` vuelve a admitir duplicados.

### Para profundizar
- [Descripción de la normalización de bases de datos (Microsoft Learn)](https://learn.microsoft.com/office/troubleshoot/access/database-normalization-description)
- [Codd, "A Relational Model of Data for Large Shared Data Banks" (ACM, 1970)](https://dl.acm.org/doi/10.1145/362384.362685)

## Subtemas

- [[Primera forma normal (1FN)]]
- [[Segunda forma normal (2FN)]]
- [[Tercera forma normal (3FN)]]
- [[Forma normal de Boyce-Codd (FNBC)]]
- [[Desnormalización deliberada]]

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Normaliza esta tabla plana hasta 3FN: `VentasPlano(PedidoId, Fecha, ClienteNombre, ClienteCiudad, ProductoNombre, Categoria, Cantidad, PrecioUnitario)`.
> - `Clientes(ClienteId PK, Nombre, Ciudad)`
> - `Productos(ProductoId PK, Nombre, Categoria, Precio)` (opcional: `Categorias(CategoriaId, Nombre)`)
> - `Pedidos(PedidoId PK, ClienteId FK, Fecha)`
> - `PedidoDetalle(PedidoId FK, ProductoId FK, Cantidad, PrecioUnitario, PK(PedidoId, ProductoId))`
>
> Es justo el esquema de la base Tienda.

> [!question]- En `Empleados(EmpleadoId, Nombre, DepartamentoId, DepartamentoNombre, JefeDepto)`, ¿qué dependencia viola 3FN y cómo se corrige?
> `DepartamentoNombre` y `JefeDepto` dependen de `DepartamentoId`, no de la clave `EmpleadoId` (dependencia transitiva). Se separa `Departamentos(DepartamentoId, Nombre, JefeId)` y `Empleados` conserva solo `DepartamentoId`.

> [!question]- Una tabla `Clientes` tiene la columna `Telefonos = '33-1234-5678, 55-8765-4321'`. ¿Qué forma normal viola y cuál es el diseño correcto?
> Viola 1FN (valor no atómico, grupo repetido). Diseño: `ClienteTelefonos(ClienteId FK, Telefono, Tipo, PK(ClienteId, Telefono))`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué problema resuelve la normalización?
> Las **anomalías**: de actualización (cambiar la ciudad de un cliente en 20 filas), de inserción (no puedes registrar un producto sin venderlo) y de borrado (borrar el último pedido borra los datos del cliente). Se logra guardando cada hecho una sola vez.

> [!question]- ¿Hasta qué forma normal suele llegar un diseño OLTP?
> A la [[Tercera forma normal (3FN)]] (o FNBC). Más allá hay formas normales (4FN, 5FN) que rara vez se necesitan en sistemas de negocio.

> [!question]- ¿Guardar `PrecioUnitario` en `PedidoDetalle` (además de `Productos.Precio`) rompe la normalización?
> No: son hechos distintos. `Productos.Precio` es el precio de lista **hoy**; `PedidoDetalle.PrecioUnitario` es el precio **pactado en esa venta**. Si lo calcularas con un join, los pedidos viejos cambiarían de importe al subir precios.

## Práctica

- [[Lab 1 - Base de práctica Tienda en SQL Server]]

## Progreso de los subtemas

```base
filters:
  and:
    - file.inFolder("06-SQL-Server/01 Básico/Normalización")
    - file.name != "Normalización"
views:
  - type: table
    name: Progreso
    order:
      - file.name
      - progreso
      - nivel
```
