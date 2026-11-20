---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: JavaScript y Web
tags: [javascript-web]
---
# Objetos

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **objeto** es una colección de **propiedades**: pares clave-valor donde la clave es una cadena (o un `Symbol`) y el valor puede ser cualquier cosa, incluidas funciones (métodos). Casi todo en JavaScript que no es primitivo es un objeto: arreglos, funciones, fechas, el DOM. Los objetos se manejan **por referencia**: dos variables pueden apuntar al mismo objeto.

### Cómo funciona
```js
const sku = 'A1';
const cliente = {
  nombre: 'Ana',
  'razon-social': 'Ana SA',      // clave con guion: va entre comillas
  [sku + '_precio']: 50,         // clave calculada
  saludar() { return `Hola, ${this.nombre}`; }, // método
};

cliente.nombre;                  // acceso con punto
cliente['razon-social'];         // acceso con corchetes (claves dinámicas o especiales)
cliente.telefono;                // undefined: no existe
cliente.direccion?.cp;           // undefined sin error (encadenamiento opcional)
delete cliente.A1_precio;        // quita la propiedad
```

Atajos: `{ nombre, rfc }` equivale a `{ nombre: nombre, rfc: rfc }`. Cada propiedad tiene además atributos internos (`writable`, `enumerable`, `configurable`), que puedes ver con `Object.getOwnPropertyDescriptor`.

**Referencias**: `const b = a` no copia; ambas variables ven el mismo objeto. Para copiar, usa spread (superficial, ver [[Spread y rest]]) o `structuredClone` (profunda, ver [[structuredClone y copia profunda]]).

### `in` vs `Object.hasOwn`
- **`'a' in obj`**: `true` si la propiedad existe **en el objeto o en su cadena de prototipos**. `'toString' in {}` es `true` (lo hereda de `Object.prototype`).
- **`Object.hasOwn(obj, 'a')`** (ES2022): `true` solo si es una propiedad **propia**. Reemplaza a `obj.hasOwnProperty('a')`, que falla con objetos sin prototipo (`Object.create(null)`) o si alguien sobrescribe `hasOwnProperty`.
- `obj.a !== undefined` no distingue entre "no existe" y "existe con valor `undefined`".

La herencia se explica en [[Prototipos y herencia prototípica]].

### Recorrer
`Object.keys(obj)`, `Object.values(obj)` y `Object.entries(obj)` devuelven arreglos de las propiedades **propias y enumerables** con clave de cadena. `Object.fromEntries(pares)` hace lo inverso:

```js
const precios = { A1: 50, B2: 120 };
const conIva = Object.fromEntries(
  Object.entries(precios).map(([k, v]) => [k, v * 1.16])
);
```

**Orden de las claves**: no es puramente de inserción. La especificación define: primero las claves que son **índices enteros** (como `'1'`, `'2'`) en **orden numérico ascendente**; después las **demás cadenas en orden de inserción**; los `Symbol` van aparte (no salen en `Object.keys`).

```js
Object.keys({ b: 1, a: 2, 2: 'x', 1: 'y' }); // ['1', '2', 'b', 'a']
```

Si necesitas orden de inserción garantizado con claves numéricas, usa un `Map` (ver [[Map y Set]]).

### `Object.freeze`
Vuelve el objeto **inmutable a primer nivel**: no se pueden agregar, quitar ni cambiar propiedades (en modo estricto, intentarlo lanza `TypeError`; en no estricto, se ignora en silencio). Pero **no es profundo**: los objetos anidados siguen siendo modificables.

```js
const config = Object.freeze({ api: '/api', reintentos: { max: 3 } });
config.api = '/otra';          // TypeError en módulos (estrictos)
config.reintentos.max = 10;    // funciona: el anidado no está congelado
Object.isFrozen(config);       // true
```

Para congelar a fondo hay que recorrer y congelar cada nivel. Variantes más suaves: `Object.seal` (no agregar ni quitar, sí cambiar) y `Object.preventExtensions` (solo impide agregar).

### Ejemplo
Normalizar la respuesta de la API para la vista de Itzamna.Clio:

```js
function aVista(dto) {
  const { id, cliente, renglones = [], ...resto } = dto;
  return Object.freeze({
    id,
    cliente: cliente?.nombre ?? 'Sin cliente',
    total: renglones.reduce((s, r) => s + r.cant * r.precio, 0),
    tieneNotas: Object.hasOwn(resto, 'notas'),
  });
}
```

### Cuándo usarlo y cuándo no
Objetos para registros con forma conocida, opciones de funciones y DTOs. `Map` para diccionarios dinámicos. Clases cuando necesitas muchas instancias con comportamiento compartido (ver [[Clases en JS]]).

### Errores comunes
- **Creer que `const` congela el objeto**: solo impide reasignar la variable.
- **Asumir orden de inserción** con claves numéricas.
- **Usar `in` para validar datos propios** y que lo heredado dé `true`.
- **Esperar que `Object.freeze` sea profundo**.
- **Comparar objetos con `===`** esperando comparación de contenido.

### Para profundizar
- [Trabajar con objetos (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_objects)
- [`Object.hasOwn()` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwn)
- [`Object.freeze()` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- ¿Qué imprime este código?
> ```js
> console.log(Object.keys({ b: 2, a: 1, 2: 'x', 1: 'y' }).join());
> ```
>
> > [!success]- Respuesta
> > `1,2,b,a`.

> [!question]- ¿Qué imprime este código?
> ```js
> const k1 = {}, k2 = {}, m = {};
> m[k1] = 'uno'; m[k2] = 'dos';
> console.log(m[k1]);
> ```
>
> > [!success]- Respuesta
> > `dos`: las claves de objeto se convierten a string `"[object Object]"`, ambas son la misma clave. Para claves objeto usa `Map`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre `'a' in obj` y `Object.hasOwn(obj, 'a')`?
> `in` busca también en la cadena de prototipos; `Object.hasOwn` solo en las propiedades propias. Los dos dan `true` si la propiedad existe aunque valga `undefined`.

> [!question]- ¿Qué hace `Object.freeze` y qué no?
> Impide añadir, quitar o cambiar propiedades del objeto (en modo estricto lanza error; si no, se ignora en silencio). Es **superficial**: los objetos anidados siguen modificables.

> [!question]- ¿En qué orden devuelve `Object.keys` las claves?
> Primero las claves tipo entero en orden ascendente, luego las de texto en orden de inserción.

