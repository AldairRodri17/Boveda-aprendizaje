---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Fundamentos
tags: [fundamentos]
contenido: completo
---
# Complejidad algorítmica (Big O)

Parte de: [[02-Fundamentos/08 Computación general/_Índice Computación general|Computación general]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **notación Big O** describe **cómo crece el costo de un algoritmo** (tiempo o memoria) a medida que crece el tamaño de la entrada `n`, normalmente en el **peor caso**. No mide segundos: mide la **tasa de crecimiento**, ignorando constantes y términos menores. Sirve para predecir si algo que funciona con 100 registros va a aguantar 10 millones, y para comparar enfoques antes de escribir código.

### Cómo funciona
Para obtener la complejidad: cuenta cuántas veces se ejecuta la operación dominante en función de `n`, quédate con el término que crece más rápido y quita las constantes. `3n² + 10n + 7` es **O(n²)**; `2n` es **O(n)**.

Reglas prácticas:
- **Secuencia**: O(a) seguido de O(b) da O(a + b), y te quedas con el mayor. Ordenar (O(n log n)) y luego recorrer (O(n)) da **O(n log n)**.
- **Anidación**: un bucle de `n` dentro de otro de `n` es O(n · n) = **O(n²)**. Si el interno es de `m` elementos distintos, O(n · m).
- **Dividir a la mitad** en cada paso (`n = n / 2` hasta 1) da **O(log n)**: con 1,000,000 son unas 20 iteraciones.
- Llamar a un método **también cuenta**: `lista.Contains(x)` en una `List<T>` es O(n) porque recorre la lista.

Clases comunes, de mejor a peor:

| Notación | Nombre | Ejemplo en .NET |
|---|---|---|
| O(1) | constante | `dict[clave]`, `HashSet.Contains`, acceso por índice en arreglo |
| O(log n) | logarítmica | búsqueda binaria, `SortedSet.Contains`, búsqueda en índice B-tree |
| O(n) | lineal | `List.Contains`, `foreach`, `Where(...).ToList()` |
| O(n log n) | linealítmica | `OrderBy`, `Array.Sort` |
| O(n²) | cuadrática | dos bucles anidados sobre la misma colección |
| O(2ⁿ) | exponencial | Fibonacci recursivo ingenuo, probar todos los subconjuntos |

**Estimar tiempos**: si un algoritmo es O(n²) y multiplicas `n` por `k`, el tiempo se multiplica por aproximadamente `k²`. Por ejemplo, si con 5,000 elementos tarda 2 s, con 20,000 (×4) tardará cerca de 2 × 16 = 32 s. Para O(n log n), ×4 en `n` es poco más de ×4 en tiempo.

**Big O no lo es todo.** Un O(n) puede ser más lento que un O(n²) para `n` pequeños si su constante es enorme (por ejemplo, crear un `Dictionary` para buscar en 5 elementos cuesta más que recorrerlos). Además influyen la caché del CPU, las asignaciones de memoria y la E/S. Big O dice qué pasa **cuando n crece**; para tamaños concretos, **mide** (BenchmarkDotNet).

### Ejemplo
Cruce de pedidos con clientes en Itzamna.Dionisio:

```csharp
// O(n · m): por cada pedido recorre toda la lista de clientes
var activos = clientes.Where(c => c.Activo).Select(c => c.Id).ToList();
var pedidosDeActivos = pedidos.Where(p => activos.Contains(p.ClienteId)).ToList();

// O(n + m): construye un HashSet una vez (O(m)) y cada búsqueda es O(1) promedio
var activosSet = clientes.Where(c => c.Activo).Select(c => c.Id).ToHashSet();
var pedidosDeActivos2 = pedidos.Where(p => activosSet.Contains(p.ClienteId)).ToList();
```

Con 100,000 pedidos y 50,000 clientes, el primero hace hasta 5,000,000,000 comparaciones; el segundo, unas 150,000 operaciones.

**Diccionario:** buscar por clave en `Dictionary<TKey,TValue>` es O(1) promedio; buscar un **valor** sin conocer la clave (`dict.ContainsValue(v)` o `dict.Values.Contains(v)`) es O(n) porque recorre todo.

**Consultas N+1:** con un ORM o con Dapper mal usado, traes `n` pedidos (1 consulta) y luego, dentro de un `foreach`, consultas el cliente de cada uno (n consultas más): **n + 1 viajes a la base**, es decir, O(n) en número de consultas, cada una con latencia de red. Con 500 pedidos y 2 ms por viaje ya son más de 1 s. La solución es una sola consulta con `JOIN`, o traer los clientes con `WHERE Id IN @ids` (Dapper expande la lista) y unirlos en memoria con un diccionario.

```sql
-- ObtenerPedidosConCliente.sql: una sola consulta en lugar de N+1
SELECT p.Id, p.Folio, p.Total, c.Id AS ClienteId, c.Nombre
FROM dbo.Pedidos p
JOIN dbo.Clientes c ON c.Id = p.ClienteId
WHERE p.Fecha >= @Desde;
```

### Cuándo usarlo y cuándo no
- Piensa en Big O al trabajar con **colecciones grandes**, bucles anidados, consultas dentro de bucles y al elegir estructuras de datos (ver [[Estructuras de datos básicas]]).
- No optimices O(n²) que solo corre con 20 elementos: prioriza claridad ([[DRY, KISS y YAGNI]]).
- Considera también la **complejidad espacial**: el `HashSet` del ejemplo cuesta memoria O(m).

### Errores comunes
- **Métodos O(n) escondidos en bucles**: `List.Contains`, `IndexOf`, `Remove`, `Count()` sobre `IEnumerable`, `string +=` en un bucle.
- **Ejecutar un `IEnumerable` varias veces** (LINQ diferido): cada recorrido repite la consulta.
- **Confundir promedio y peor caso**: una tabla hash es O(1) promedio pero O(n) con colisiones masivas.
- **Creer que O(n) siempre gana**: mide con datos reales.
- **Ignorar la base de datos**: un `WHERE` sin índice es un recorrido O(n) de la tabla.

### Para profundizar
- [Big-O Cheat Sheet](https://www.bigocheatsheet.com/)
- [Colecciones en .NET y su complejidad (Microsoft Learn)](https://learn.microsoft.com/dotnet/standard/collections/)
- [BenchmarkDotNet](https://benchmarkdotnet.org/)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- ¿Complejidad de dos `for` anidados de 0 a n?
> **O(n²)**.

> [!question]- ¿Complejidad de un bucle que divide `n` entre 2 hasta llegar a 1?
> **O(log n)**.

> [!question]- ¿Complejidad de ordenar con `OrderBy` y luego recorrer una vez?
> O(n log n) + O(n) = **O(n log n)**.

> [!question]- Un algoritmo O(n²) tarda 1 s con n = 10,000. ¿Cuánto con n = 100,000?
> ≈ 100 s (10× la entrada → 100× el tiempo).

> [!question]- ¿Complejidad de buscar una clave en `Dictionary` y de buscar un valor sin conocer la clave?
> O(1) promedio por clave; O(n) por valor (hay que recorrer).

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué mide la notación Big O?
> Cómo crece el tiempo (o la memoria) de un algoritmo en el **peor caso** cuando crece la entrada, ignorando constantes: O(1), O(log n), O(n), O(n log n), O(n²), O(2ⁿ).

> [!question]- ¿Por qué `lista.Contains(x)` dentro de un `foreach` sobre otra lista puede ser un problema?
> Es O(n·m): con 100,000 × 100,000 son 10¹⁰ comparaciones. Convertir la segunda a `HashSet` lo baja a O(n + m).

> [!question]- ¿Un algoritmo O(n) siempre es más rápido que uno O(n²)?
> No para entradas pequeñas: las constantes y la caché importan. Big O describe el crecimiento; para n chicos hay que medir.

> [!question]- ¿Qué complejidad tienen las consultas N+1 en un ORM?
> 1 consulta para la lista + N consultas para los detalles: el número de viajes a la BD crece linealmente con los resultados; se resuelve con `Include`, proyecciones o una consulta por lotes.

