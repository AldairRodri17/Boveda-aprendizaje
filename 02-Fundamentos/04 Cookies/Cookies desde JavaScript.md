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
# Cookies desde JavaScript

Parte de: [[02-Fundamentos/04 Cookies/_Índice Cookies|Cookies]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El navegador expone las cookies a JavaScript con dos APIs:
- **`document.cookie`**: la API clásica, síncrona y con una interfaz peculiar basada en cadenas.
- **Cookie Store API** (`cookieStore`): API moderna, asíncrona (con promesas) y con eventos de cambio.

Desde JS solo se ven las cookies **no `HttpOnly`** que aplican a la página actual (por dominio y ruta). Es útil para preferencias del front; no para sesiones.

### Cómo funciona
**Leer.** `document.cookie` devuelve una sola cadena con los pares visibles: `"tema=oscuro; idioma=es-MX"`. No incluye atributos (ni vencimiento, ni dominio). Hay que parsearla.

**Escribir.** Asignar a `document.cookie` **no reemplaza** todas las cookies: crea o actualiza **una**, con la sintaxis de `Set-Cookie`:

```js
document.cookie = "tema=oscuro; Max-Age=31536000; Path=/; SameSite=Lax; Secure";
```

**Borrar.** Asignar la misma cookie (mismo nombre, `Path` y `Domain`) con `Max-Age=0`.

**Qué no se ve:**
- Cookies `HttpOnly` (por diseño, ver [[Cookies - Secure y HttpOnly]]).
- Cookies de otros dominios o de rutas que no coinciden con la página.
- JS no puede crear cookies `HttpOnly`.

**Cookie Store API.** `await cookieStore.get('tema')` devuelve un objeto con nombre, valor y algunos atributos; `cookieStore.set({...})` escribe; `cookieStore.addEventListener('change', ...)` avisa de cambios. Disponibilidad: Chrome y Edge desde la versión 87 y Firefox desde la 140 (junio de 2025); según el explorador de *web features*, Safari aún no la soporta de forma completa, por lo que no es Baseline (Pendiente de confirmar al momento de usarla). Si la usas, detecta la característica y deja `document.cookie` como respaldo.

### Ejemplo
Utilidades en JavaScript ES2022 sin bundler para Itzamna.Clio (archivo `cookies.js` cargado como módulo):

```js
// cookies.js — utilidades mínimas para cookies NO sensibles
export function leerCookie(nombre) {
  const prefijo = encodeURIComponent(nombre) + "=";
  for (const parte of document.cookie.split("; ")) {
    if (parte.startsWith(prefijo)) {
      return decodeURIComponent(parte.slice(prefijo.length));
    }
  }
  return null;
}

export function escribirCookie(nombre, valor, { dias = 30, path = "/" } = {}) {
  const maxAge = Math.round(dias * 24 * 60 * 60); // segundos
  document.cookie =
    `${encodeURIComponent(nombre)}=${encodeURIComponent(valor)}` +
    `; Max-Age=${maxAge}; Path=${path}; SameSite=Lax; Secure`;
}

export function borrarCookie(nombre, { path = "/" } = {}) {
  document.cookie = `${encodeURIComponent(nombre)}=; Max-Age=0; Path=${path}`;
}

// Uso
escribirCookie("idioma", "es-MX", { dias: 30 }); // Max-Age=2592000
console.log(leerCookie("idioma"));               // "es-MX"
borrarCookie("idioma");
```

Versión con Cookie Store API y respaldo:

```js
export async function obtenerIdioma() {
  if ("cookieStore" in globalThis) {
    const c = await cookieStore.get("idioma");
    return c?.value ?? null;
  }
  return leerCookie("idioma");
}
```

En React + TypeScript puedes envolverlo en un hook, pero el principio no cambia: el front solo maneja cookies que no son secretas.

Recuerda que `Secure` en `document.cookie` hace que en `http://` (excepto `localhost`) la cookie no se guarde; en desarrollo con Vite en `http://localhost:5173` sí funciona.

### Cuándo usarlo y cuándo no
- **Sí**: preferencias de interfaz que el servidor también lee al renderizar (idioma, tema), banderas de consentimiento, leer el token antiforgery en el patrón *double-submit* (ver [[Cookies y CSRF]]).
- **No**: tokens de acceso o refresh, ids de sesión, datos personales. Si el front necesita algo que no debe viajar al servidor, usa `localStorage` o IndexedDB (ver [[Cookies vs localStorage vs sessionStorage]]).

### Errores comunes
1. **Parsear con `split(";")` sin quitar espacios.** El separador real es `"; "`; con `";"` el nombre queda con un espacio delante y no coincide.
2. **No codificar el valor.** Un valor con `;` o `,` corta la cookie. Usa `encodeURIComponent`.
3. **Borrar sin el mismo `Path`.** Si se creó con `Path=/app`, bórrala con `Path=/app`.
4. **Intentar leer la cookie de sesión.** Si es `HttpOnly` no aparece, y está bien. Si "necesitas" leerla, pide al servidor un endpoint como `/api/yo` que devuelva el usuario.
5. **Esperar atributos en `document.cookie`.** Solo hay nombre y valor; el vencimiento no se puede consultar ahí.

### Para profundizar
- [MDN: Document.cookie](https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie)
- [MDN: Cookie Store API](https://developer.mozilla.org/en-US/docs/Web/API/Cookie_Store_API)
- [Web features explorer: Cookie Store (soporte por navegador)](https://web-platform-dx.github.io/web-features-explorer/features/cookie-store)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Escribe una función que obtenga el valor de una cookie por nombre.
> ```js
> function getCookie(nombre) {
>   const par = document.cookie.split('; ').find(c => c.startsWith(nombre + '='));
>   return par ? decodeURIComponent(par.slice(nombre.length + 1)) : null;
> }
> ```

> [!question]- Guarda `idioma=es-MX` por 30 días para todo el sitio.
> `document.cookie = 'idioma=' + encodeURIComponent('es-MX') + '; Max-Age=2592000; Path=/; SameSite=Lax';`

> [!question]- Borra la cookie `idioma`.
> `document.cookie = 'idioma=; Max-Age=0; Path=/';`

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo se leen y escriben cookies desde JS?
> `document.cookie` devuelve las cookies visibles como `"a=1; b=2"`. Asignar `document.cookie = "tema=oscuro; Max-Age=31536000; Path=/; SameSite=Lax"` crea o actualiza **una** cookie (no reemplaza todas).

> [!question]- ¿Qué cookies no ves en `document.cookie`?
> Las `HttpOnly` y las de otros dominios o rutas que no aplican a la página actual.

> [!question]- ¿Qué alternativa moderna hay a `document.cookie`?
> La **Cookie Store API** (`cookieStore.get/set`, asíncrona y con eventos de cambio). Su disponibilidad varía por navegador (Pendiente de confirmar soporte actual en Firefox y Safari).

## Práctica

- [[Lab 1 - Cookies y CORS en el navegador con DevTools]]

