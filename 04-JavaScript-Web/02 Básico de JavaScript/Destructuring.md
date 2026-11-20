---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: JavaScript y Web
tags: [javascript-web]
---
# Destructuring

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **desestructuración** (*destructuring assignment*) es una sintaxis para **extraer valores de arreglos y propiedades de objetos en variables** con una sola expresión que imita la forma del dato. Reduce código repetitivo (`const nombre = cliente.nombre; const rfc = cliente.rfc;`) y es omnipresente en React (props, `useState`) y en funciones que reciben objetos de opciones.

### Cómo funciona
**Objetos**: se extrae por **nombre de propiedad**; el orden no importa.

```js
const cliente = { id: 7, nombre: 'Ana', direccion: { cp: '06600' } };

const { nombre, id } = cliente;                // nombre = 'Ana', id = 7
const { nombre: nombreCliente } = cliente;     // renombrar: variable nombreCliente
const { telefono = 'sin teléfono' } = cliente; // valor por defecto
const { direccion: { cp } } = cliente;         // anidado: cp = '06600'
const { id: _, ...resto } = cliente;           // resto = { nombre, direccion }
```

La sintaxis para renombrar es `propiedad: nuevoNombre` (al revés de lo que mucha gente espera). Se combina con defecto: `{ telefono: tel = 'N/D' }`.

**Arreglos**: se extrae por **posición**.

```js
const [primero, segundo] = ['a', 'b', 'c'];    // 'a', 'b'
const [, , tercero] = ['a', 'b', 'c'];         // saltar con comas: 'c'
const [x = 0, y = 0] = [5];                    // x = 5, y = 0
const [cabeza, ...cola] = [1, 2, 3];           // cola = [2, 3]
```

Funciona con cualquier iterable: `const [a, b] = 'hi'`, `for (const [clave, valor] of mapa)`.

### Valores por defecto: cuándo se aplican
El valor por defecto se usa **solo si el valor extraído es `undefined`** (porque falta o porque vale `undefined`). **No** se aplica con `null`, `0`, `''` ni `false`:

```js
const { a = 1 } = { a: undefined };  // a = 1
const { b = 1 } = { b: null };       // b = null
const { c = 1 } = { c: 0 };          // c = 0
```

Mismo criterio que los parámetros por defecto de funciones. Si quieres reemplazar también `null`, usa `??` después (ver [[Operadores de null y lógicos en JS]]).

### Intercambiar variables
```js
let m1 = 'Ana', m2 = 'Luis';
[m1, m2] = [m2, m1];
```

Se crea un arreglo temporal con los valores y se desestructura en orden inverso, sin variable auxiliar.

### En parámetros de función
Muy útil para "argumentos con nombre" con valores por defecto:

```js
async function buscarPedidos({ cliente, estado = 'nuevo', pagina = 1, tamano = 20 } = {}) {
  const qs = new URLSearchParams({ estado, pagina, tamano });
  if (cliente) qs.set('cliente', cliente);
  const r = await fetch(`/api/pedidos?${qs}`);
  return r.json();
}

buscarPedidos({ cliente: 7 });   // usa estado, pagina y tamano por defecto
buscarPedidos();                 // el "= {}" final evita TypeError sin argumentos
```

En React:

```jsx
function TarjetaPedido({ folio, total, onSurtir }) { /* ... */ }
const [pedidos, setPedidos] = useState([]);
```

### Ejemplo con la respuesta de una API
La API de Itzamna.Dionisio devuelve un ProblemDetails con `codigo`:

```js
const r = await fetch('/api/pedidos', { method: 'POST', body });
if (!r.ok) {
  const { title, status, codigo = 'DESCONOCIDO', errors = {} } = await r.json();
  for (const [campo, mensajes] of Object.entries(errors)) {
    marcarError(campo, mensajes[0]);
  }
}
```

### Cómo pensarlo
Lee el patrón de la izquierda como un **molde** con la misma forma que el dato: las llaves dicen "de este objeto saca estas propiedades" y los corchetes dicen "de esta lista toma estas posiciones". El motor no copia el objeto completo: solo lee las propiedades nombradas y las asigna a variables nuevas. Por eso desestructurar no altera el original, y si una propiedad es un objeto, la variable apunta **al mismo** objeto (no es una copia).

Tres reglas que conviene memorizar:
1. En objetos manda el **nombre**; en arreglos manda la **posición**.
2. El valor por defecto solo entra con `undefined`.
3. El patrón *rest* (`...resto`) va al final y recoge lo que no nombraste, lo cual sirve para **quitar** campos (por ejemplo, separar `password` antes de mostrar un usuario).

Combinarlo con `Object.entries` y `Map` hace muy legibles los recorridos: `for (const [sku, cantidad] of inventario)` dice exactamente qué contiene cada par, sin `par[0]` ni `par[1]`.

### Cuándo usarlo y cuándo no
Úsala para tomar pocas propiedades de un objeto, en parámetros de opciones, al iterar `Object.entries` o un `Map`, y con funciones que devuelven pares (`useState`). Evita desestructuraciones anidadas muy profundas: si falta un nivel intermedio, lanza `TypeError` (`Cannot read properties of undefined`); en datos opcionales, mejor `obj?.a?.b`.

### Errores comunes
- **Desestructurar `undefined` o `null`**: `const { a } = undefined` lanza `TypeError`. Pon un defecto `= {}` en parámetros.
- **Confundir la sintaxis de renombrar** (`{ nuevo: viejo }` está al revés).
- **Esperar que el defecto reemplace `null`**.
- **Asignación a variables ya declaradas con objetos sin paréntesis**: `{ a } = obj;` es error de sintaxis porque `{` se interpreta como bloque; escribe `({ a } = obj);`.

### Para profundizar
- [Asignación por desestructuración (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring)
- [Parámetros por defecto (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- ¿Qué imprime este código?
> ```js
> const { a = 1, b: { c } = { c: 9 }, ...rest } = { b: { c: 2 }, d: 3, e: 4 };
> console.log(a, c, rest);
> ```
>
> > [!success]- Respuesta
> > `1 2 { d: 3, e: 4 }`.

> [!question]- ¿Qué imprime este código?
> ```js
> const [x, , y = 7, ...zs] = [1, 2, undefined, 4, 5];
> console.log(x, y, zs);
> ```
>
> > [!success]- Respuesta
> > `1 7 [4, 5]`.

> [!question]- Intercambia `m1` y `m2` en una línea.
> `[m1, m2] = [m2, m1];`

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es la desestructuración?
> Sintaxis para extraer valores de arreglos (por posición) u objetos (por nombre) en variables: `const { id, nombre } = usuario; const [a, b] = par;`.

> [!question]- ¿Cuándo se aplica un valor por defecto en desestructuración?
> Solo cuando el valor es `undefined`, no con `null`.

> [!question]- ¿Cómo renombras al desestructurar?
> `const { nombre: n } = usuario;` crea la variable `n`.

