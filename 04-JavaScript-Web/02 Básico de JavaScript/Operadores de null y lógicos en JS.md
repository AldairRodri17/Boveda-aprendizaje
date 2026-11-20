---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: JavaScript y Web
aliases: ["Operadores (??, ?., ||)"]
tags: [javascript-web]
---
# Operadores de null y lógicos en JS

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
JavaScript tiene operadores para combinar condiciones y manejar valores ausentes:

- **Lógicos**: `&&` (y), `||` (o), `!` (no).
- **Coalescencia nula** `??`: "usa el de la derecha si el de la izquierda es `null` o `undefined`".
- **Encadenamiento opcional** `?.`: "accede, pero si lo de la izquierda es `null`/`undefined`, detente y da `undefined`".
- **Asignación lógica** `&&=`, `||=`, `??=` (ES2021).

### Cómo funcionan `&&` y `||`
No devuelven `true`/`false` necesariamente: devuelven **uno de sus operandos**, y hacen **cortocircuito** (no evalúan el lado derecho si no hace falta).

- **`a || b`**: si `a` es *truthy*, devuelve `a`; si no, devuelve `b`.
- **`a && b`**: si `a` es *falsy*, devuelve `a`; si no, devuelve `b`.

En cadenas, `&&` devuelve el **primer falsy** (o el último si todos son truthy) y `||` el **primer truthy** (o el último si todos son falsy):

```js
1 && 2 && 3           // 3
1 && 0 && 3           // 0
false || null || ''   // ''   (todos falsy: devuelve el último)
usuario && usuario.nombre    // patrón antiguo de acceso seguro
```

Truthy y falsy se explican en [[Valores truthy y falsy]]. `!x` convierte a booleano y lo niega; `!!x` lo convierte sin negar.

### `||` vs `??`
La diferencia está en qué valores se reemplazan:
- `||` reemplaza **cualquier falsy**: `0`, `''`, `false`, `NaN`, `null`, `undefined`.
- `??` reemplaza **solo `null` y `undefined`**.

```js
0 || 'x'       // 'x'
0 ?? 'x'       // 0
'' ?? 'd'      // ''
null ?? 'd'    // 'd'
```

Esto importa con valores válidos que son falsy: una cantidad `0`, un descuento `0`, un texto vacío intencional, `false` en una opción.

```js
const pagina = parametros.pagina || 1;   // si pagina es 0... se vuelve 1 (bug si 0 era válido)
const descuento = pedido.descuento ?? 0; // respeta 0, solo rellena si falta
const mostrarIva = opciones.mostrarIva ?? true; // respeta false
```

`??` no se puede mezclar directamente con `&&` o `||` sin paréntesis: `a || b ?? c` es error de sintaxis; escribe `(a || b) ?? c`.

### Encadenamiento opcional `?.`
- **`obj?.a?.b`**: si `obj` es `null`/`undefined`, devuelve `undefined` sin evaluar el resto; si no, lee `a`, y lo mismo con `b`. Evita `TypeError: Cannot read properties of undefined`.
- **`obj?.[clave]`**: acceso con corchetes.
- **`fn?.()`**: llama a `fn` solo si existe (no es `null`/`undefined`). Útil con callbacks opcionales: `opciones.onExito?.(datos)`.

```js
const cp = pedido?.cliente?.direccion?.cp ?? 'Sin CP';
```

Solo protege contra `null`/`undefined`, no contra otros tipos: si `fn` es un número, `fn?.()` igual lanza `TypeError`. Y no lo uses en todo: si un dato **debe** existir, que falle ruidosamente en lugar de esconder un error con `undefined`.

### Asignación lógica
```js
opciones.reintentos ??= 3;   // asigna solo si es null/undefined
estado.titulo ||= 'Sin título'; // asigna si es falsy
usuario.activo &&= validar(usuario); // asigna solo si es truthy
```

Equivalen a `a ?? (a = b)`, etc.: **solo asignan si hace falta**, lo que importa con *setters* o propiedades de solo lectura.

### Ejemplo
Leer configuración con defectos en Itzamna.Clio:

```js
export function crearCliente(opciones = {}) {
  const base = opciones.baseUrl ?? '/api';
  const timeoutMs = opciones.timeoutMs ?? 10_000;   // permite 0 si alguien lo pasa a propósito
  const onError = opciones.onError;

  return async (ruta) => {
    try {
      const r = await fetch(base + ruta, { signal: AbortSignal.timeout(timeoutMs) });
      return r.ok && (await r.json());               // false si no fue ok
    } catch (e) {
      onError?.(e);                                  // callback opcional
      return null;
    }
  };
}
```

### Cuándo usarlo y cuándo no
**`??` para valores por defecto** cuando 0, '' o false son válidos (casi siempre). `||` cuando de verdad quieres reemplazar cualquier falsy (por ejemplo, un texto vacío por un placeholder). `?.` para datos **opcionales** según el contrato de la API. `&&` en JSX para renderizar condicionalmente, cuidando que el lado izquierdo no sea `0` (React lo pintaría).

### Errores comunes
- **`||` con números o booleanos válidos**: convierte 0 o false en el defecto.
- **`{cantidad && <Badge />}` en React con cantidad 0**: muestra "0". Usa `cantidad > 0 &&`.
- **Abusar de `?.`** y ocultar datos que deberían existir.
- **Mezclar `??` con `||` sin paréntesis**.

### Para profundizar
- [Coalescencia nula `??` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing)
- [Encadenamiento opcional `?.` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining)
- [OR lógico `||` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_OR)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Predice: `0 || 'x'`, `0 ?? 'x'`, `'' ?? 'd'`, `null ?? 'd'`.
> `"x"`, `0`, `""`, `"d"`.

> [!question]- Predice: `1 && 2 && 3`, `1 && 0 && 3`, `false || null || ''`.
> `3`, `0`, `""`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre `a || b` y `a ?? b`?
> `||` devuelve `b` si `a` es **falsy** (incluye 0 y ""). `??` devuelve `b` solo si `a` es `null` o `undefined`. Para valores por defecto donde 0 o "" son válidos, usa `??`.

> [!question]- ¿Qué hace `obj?.a?.b` y `fn?.()`?
> Encadenamiento opcional: si lo de la izquierda es `null`/`undefined`, corta y devuelve `undefined` sin lanzar error. `fn?.()` solo llama si `fn` existe.

> [!question]- ¿Qué devuelven `&&` y `||` realmente?
> Uno de los operandos, no necesariamente un booleano. `&&` devuelve el primero falsy o el último; `||` el primero truthy o el último.

> [!question]- ¿Qué hacen `??=`, `||=` y `&&=`?
> Asignación lógica: `a ??= 5` asigna solo si `a` es null/undefined; `a ||= 5` si es falsy; `a &&= 5` si es truthy.

