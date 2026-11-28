---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: JavaScript y Web
tags: [javascript-web]
contenido: completo
---
# Manejo de errores en JS

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El manejo de errores en JavaScript se basa en **excepciones**: cuando algo falla, se **lanza** (`throw`) un valor, normalmente un objeto `Error`, que interrumpe la ejecución normal y sube por la pila de llamadas hasta que un bloque `try/catch` lo **atrapa**. Si nadie lo atrapa, termina en la consola como "Uncaught" (o, en Node, puede terminar el proceso).

### Cómo funciona
```js
try {
  const datos = JSON.parse(texto);   // puede lanzar SyntaxError
  procesar(datos);
} catch (error) {                    // se ejecuta solo si algo lanzó
  console.error('No se pudo procesar', error);
} finally {                          // se ejecuta SIEMPRE, haya error o no
  ocultarSpinner();
}
```

**Tipos nativos**: `Error` (base), `TypeError` (operación sobre un tipo incorrecto, como leer una propiedad de `undefined`), `ReferenceError` (variable inexistente o en TDZ), `SyntaxError` (por ejemplo, en `JSON.parse`), `RangeError` (valor fuera de rango), `AggregateError` (varios errores, lo usa `Promise.any`). Cada error tiene `name`, `message`, `stack` y opcionalmente `cause`.

**`finally`** corre al salir del `try` o del `catch` por cualquier camino: terminación normal, `return`, `break` o una excepción. Sirve para **liberar recursos y restaurar estado** (cerrar un spinner, habilitar un botón, soltar un candado). Ojo: si `finally` hace `return`, ese valor **reemplaza** al del `try` o incluso **traga** la excepción; evita `return` en `finally`.

```js
function f() {
  try { return 'try'; }
  finally { console.log('finally'); }
}
f(); // imprime "finally" y devuelve "try"
```

### Qué atrapa y qué no
`try/catch` atrapa errores **síncronos** lanzados mientras se ejecuta su bloque, y errores de promesas **si las esperas con `await`** dentro del `try`. **No** atrapa:

- Errores de **sintaxis** del propio archivo (el archivo no llega a ejecutarse).
- Errores en **callbacks que se ejecutan después**, cuando el `try` ya terminó: `try { setTimeout(() => { throw e }) } catch {}` no atrapa nada.
- **Promesas rechazadas sin `await`**: `try { fetch(url) } catch {}` no ve el rechazo. Usa `await` o `.catch()`.

```js
async function cargarPedido(id) {
  try {
    const r = await fetch(`/api/pedidos/${id}`); // await: el rechazo llega al catch
    if (!r.ok) throw new ErrorApi(r.status, await r.json());
    return await r.json();
  } catch (e) {
    if (e instanceof ErrorApi && e.status === 404) return null;
    throw e;                                      // relanza lo que no sabes manejar
  }
}
```

Para lo que se escapa, existen redes de seguridad globales: `window.addEventListener('error', ...)` y `'unhandledrejection'` en el navegador, y `process.on('unhandledRejection', ...)` en Node. Úsalas para **registrar** (por ejemplo, enviar a Application Insights), no para "arreglar".

### Errores propios
Extiende `Error` para distinguir tipos con `instanceof` y llevar datos extra. Tu API de Itzamna.Dionisio responde ProblemDetails con `codigo`:

```js
export class ErrorApi extends Error {
  constructor(status, problema, opciones) {
    super(problema?.title ?? `HTTP ${status}`, opciones); // opciones puede traer { cause }
    this.name = 'ErrorApi';
    this.status = status;
    this.codigo = problema?.codigo;
    this.errores = problema?.errors ?? {};
  }
}

try {
  await guardar(pedido);
} catch (e) {
  if (e instanceof ErrorApi && e.codigo === 'PEDIDO_DUPLICADO') avisar('Ya existe');
  else throw new Error('Falló el guardado del pedido', { cause: e }); // conserva el original
}
```

`{ cause }` (ES2022) encadena el error original para no perder la traza al envolverlo. Pon `this.name` para que la consola muestre el tipo correcto.

Lanza siempre instancias de `Error` (no cadenas: `throw 'falló'` no trae `stack`).

### El `catch` vacío
```js
try { guardar(); } catch (e) {}   // mal
```

Es malo porque **oculta el fallo**: el usuario cree que se guardó, no queda rastro en la consola ni en los logs, y el error aparece después como un síntoma raro lejos de su causa. Si de verdad un error es esperado e ignorable, atrápalo **específicamente**, deja un comentario de por qué y, como mínimo, regístralo. Si no sabes qué hacer con un error, **no lo atrapes**: deja que suba hasta una capa que sí sepa (la vista que muestra un mensaje, el manejador global).

### Cuándo usarlo y cuándo no
Usa `try/catch` en los **bordes**: llamadas a red, `JSON.parse` de datos externos, acceso a `localStorage`, y en la capa que puede mostrar un mensaje o reintentar. No envuelvas cada función: el código intermedio debe dejar pasar los errores. Para validaciones de negocio esperadas (un formulario con campos vacíos), muchas veces es más claro devolver un resultado (`{ ok: false, errores }`) que lanzar.

### Errores comunes
- **`catch` vacío** o que solo hace `console.log` y sigue como si nada.
- **Olvidar `await`** dentro del `try`.
- **Esperar que `fetch` lance en 404/500**: solo rechaza por fallas de red o abortos; revisa `r.ok`.
- **Perder la causa** al envolver errores sin `cause`.
- **`return` en `finally`**.

### Para profundizar
- [Control de flujo y manejo de errores (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Control_flow_and_error_handling)
- [`try...catch` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch)
- [`Error` y `cause` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- ¿Qué imprime este código?
> ```js
> async function a() { try { return Promise.reject(new Error('r')); } catch { return 'atrapado'; } }
> async function b() { try { return await Promise.reject(new Error('r')); } catch { return 'atrapado'; } }
> console.log(await a().catch(() => 'escapó'), await b());
> ```
>
> > [!success]- Respuesta
> > `escapó atrapado`: sin `await`, la promesa rechazada sale del `try` antes de rechazarse.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué atrapa un `try/catch` y qué no?
> Atrapa errores lanzados de forma **síncrona** dentro del bloque, o promesas rechazadas si las esperas con `await` dentro del `try`. No atrapa errores dentro de un callback que se ejecuta después (un `setTimeout` o un `.then` sin `await`).

> [!question]- ¿Cómo creas tus propios tipos de error?
> Extiende `Error`: `class HttpError extends Error { constructor(status, msg) { super(msg); this.name = 'HttpError'; this.status = status; } }`. Puedes encadenar la causa con `new Error('msg', { cause: err })`.

> [!question]- ¿Para qué sirve `finally`?
> Código que corre siempre, haya o no error (quitar un spinner, liberar un recurso).

> [!question]- Error común: `catch (e) {}` vacío. ¿Por qué es malo?
> Oculta el fallo: el usuario ve que "no pasa nada" y tú no tienes rastro. Al menos regístralo y muestra un mensaje; si no puedes manejarlo, vuelve a lanzarlo.

## Práctica

- [[Lab 4 - fetch con AbortController y manejo de errores]]

