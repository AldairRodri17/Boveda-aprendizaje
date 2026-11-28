---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: JavaScript y Web
tags: [javascript-web]
contenido: completo
---
# Condicionales y bucles en JS

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Las **condicionales** deciden qué código ejecutar (`if/else`, `switch`, operador ternario) y los **bucles** repiten código (`for`, `while`, `do...while`, `for...of`, `for...in`). Además, los arreglos tienen métodos de iteración (`forEach`, `map`, `some`…) que muchas veces reemplazan a los bucles.

### Condicionales
```js
if (pedido.total > 10000) aplicarRevision();
else if (pedido.total > 0) confirmar();
else rechazar();

const etiqueta = pedido.pagado ? 'Pagado' : 'Pendiente'; // ternario: es una expresión
```

La condición se evalúa como booleano con las reglas de [[Valores truthy y falsy]].

**`switch`** compara con `===` contra cada `case`. Al encontrar coincidencia, ejecuta desde ahí **hacia abajo** hasta un `break` o `return`. Si olvidas `break`, el código "se cae" (*fall-through*) al siguiente `case`:

```js
switch (estado) {
  case 'nuevo':
    console.log('Nuevo');      // sin break: también ejecuta lo de 'surtido'
  case 'surtido':
    console.log('Surtido');
    break;
  default:
    console.log('Otro');
}
// estado = 'nuevo' imprime "Nuevo" y "Surtido"
```

El *fall-through* intencional sirve para agrupar casos (`case 'a': case 'b': ...`). Para mapear valores, un objeto o `Map` suele ser más limpio: `const textos = { nuevo: 'Nuevo', surtido: 'Surtido' }; textos[estado] ?? 'Otro'`.

### Bucles
- **`for (let i = 0; i < n; i++)`**: control total del índice.
- **`while (cond)`** / **`do { } while (cond)`**: cuando no sabes cuántas vueltas (el `do` ejecuta al menos una).
- **`for...of`**: recorre los **valores** de cualquier **iterable**: arreglos, cadenas, `Map`, `Set`, `NodeList`, generadores.
- **`for...in`**: recorre las **claves** (nombres de propiedades enumerables, como cadenas) de un **objeto**, **incluidas las heredadas** del prototipo.

```js
const arr = ['a', 'b'];
for (const v of arr) console.log(v);   // 'a', 'b'
for (const k in arr) console.log(k);   // '0', '1' (cadenas)

const cliente = { nombre: 'Ana', rfc: 'XAXX010101000' };
for (const [k, v] of Object.entries(cliente)) console.log(k, v); // forma moderna
```

Regla práctica: **`for...of` para arreglos e iterables**; para objetos, `Object.keys/values/entries` con `for...of`. Evita `for...in` en arreglos: da índices como texto, puede incluir propiedades heredadas y no garantiza el orden que esperas en todos los casos.

`break` sale del bucle, `continue` pasa a la siguiente vuelta, y con etiquetas (`externo: for (...)`) puedes salir de bucles anidados.

### `forEach` y la salida anticipada
`forEach` llama a tu función para cada elemento y **no se puede detener**: `break` es un error de sintaxis dentro del callback y `return` solo termina esa vuelta. Opciones:

- Usa **`for...of`** con `break`.
- Usa el método que expresa la intención y se detiene solo: `find` (el primero que cumple), `some` (¿alguno?), `every` (¿todos?), `findIndex`. Ver [[find, some, every y flatMap]].
- Lanzar una excepción para salir funciona, pero es un antipatrón.

`forEach` tampoco espera promesas: `arr.forEach(async x => await guardar(x))` lanza todo en paralelo sin esperar. Para secuencial usa `for...of` con `await`; para paralelo, `Promise.all(arr.map(...))`.

### Ejemplo
Validar renglones de un pedido y detenerse en el primer error:

```js
function primerError(renglones) {
  for (const [i, r] of renglones.entries()) {
    if (r.cant <= 0) return `Renglón ${i + 1}: cantidad inválida`;
    if (!r.sku) return `Renglón ${i + 1}: falta SKU`;
  }
  return null;
}

// Equivalente declarativo
const invalido = renglones.find(r => r.cant <= 0 || !r.sku);

// Guardar en orden, uno por uno
for (const r of renglones) {
  await guardarRenglon(r);
}
```

### Cuándo usarlo y cuándo no
Prefiere métodos de arreglo (`map`, `filter`, `find`) cuando expresan mejor la intención; usa `for...of` cuando necesitas `break`, `await` secuencial o lógica compleja. Usa `for` clásico para índices o rendimiento muy fino. Evita `switch` largos que crecen con cada caso nuevo: un objeto de búsqueda o polimorfismo escala mejor.

### Errores comunes
- **Olvidar `break`** en `switch`.
- **`for...in` sobre arreglos**.
- **Intentar `break` en `forEach`**.
- **`async` dentro de `forEach`** esperando secuencialidad.
- **Modificar el arreglo mientras lo recorres** (agregar o quitar elementos): salta o repite elementos. Recorre una copia o construye uno nuevo.

### Para profundizar
- [Control de flujo y manejo de errores (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Control_flow_and_error_handling)
- [Bucles e iteración (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Loops_and_iteration)
- [`for...of` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre `for...of` y `for...in`?
> `for...of` recorre **valores** de un iterable (arreglos, strings, Map, Set). `for...in` recorre **claves** enumerables de un objeto (incluidas heredadas); no lo uses para arreglos.

> [!question]- ¿Qué pasa si olvidas `break` en un `switch`?
> La ejecución "cae" al siguiente `case` y lo ejecuta también (*fall-through*). `switch` compara con `===`.

> [!question]- ¿Cómo sales de un `forEach` antes de tiempo?
> No se puede con `break`. Usa `for...of` con `break`, o `some`/`find`/`every`, que paran solas.

