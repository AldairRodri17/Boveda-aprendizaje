---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Fundamentos
tags: [fundamentos]
contenido: completo
---
# Anatomía de una URL

Parte de: [[02-Fundamentos/01 Internet y web/_Índice Internet y web|Internet y web]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **URL** (Uniform Resource Locator) es la dirección completa de un recurso: dice con qué protocolo hablar, a qué máquina y puerto, qué recurso pedir y con qué parámetros. Entender sus partes te ayuda a diseñar endpoints, depurar redirecciones, configurar CORS y evitar errores de codificación.

### Cómo funciona
Tomemos esta URL:

```text
https://ana:pw@api.ejemplo.com:8443/v1/clientes/42?activo=true&page=2#datos
\___/   \____/ \_____________/ \__/\_____________/ \_________________/ \___/
esquema credenc.     host      puerto    ruta             query       fragmento
```

| Parte | Valor | Notas |
|---|---|---|
| Esquema | `https` | Define protocolo y puerto por defecto (`http` 80, `https` 443) |
| Credenciales | `ana:pw` | Obsoletas y peligrosas: quedan en logs e historial. No las uses |
| Host | `api.ejemplo.com` | Se resuelve con [[DNS]] |
| Puerto | `8443` | Si se omite, se usa el del esquema |
| Ruta | `/v1/clientes/42` | Identifica el recurso |
| Query | `activo=true&page=2` | Pares `clave=valor` separados por `&` |
| Fragmento | `datos` | **No se envía al servidor**; lo usa el navegador (anclas, routers por hash) |

**Origen** = esquema + host + puerto. `https://app.ejemplo.com/login?x=1` tiene origen `https://app.ejemplo.com:443` (el 443 es implícito). El origen es la unidad que usan CORS y la política de mismo origen: `http://` y `https://` del mismo host son orígenes distintos, igual que dos puertos distintos.

#### Percent-encoding
Caracteres como espacio, `&`, `=`, `?`, `#`, `/` o `%` tienen significado en la URL. Para usarlos como **dato** se codifican como `%` seguido del byte en hexadecimal (en UTF-8):

| Carácter | Codificado |
|---|---|
| espacio | `%20` (en formularios a veces `+`) |
| `&` | `%26` |
| `#` | `%23` |
| `%` | `%25` |
| `ñ` | `%C3%B1` |

Así, `/buscar?q=pan%20dulce%26cafe` llega al servidor como **un solo** parámetro `q` con valor `pan dulce&cafe`. Sin codificar, `?q=pan dulce&cafe` se partiría en `q=pan dulce` y un parámetro `cafe` vacío.

### Ejemplo
En el front (Itzamna.Clio, TypeScript) arma URLs con las APIs del navegador, no concatenando:

```ts
const url = new URL("/v1/clientes", "https://api.ejemplo.com");
url.searchParams.set("q", "50% off #hoy");   // se codifica solo
url.searchParams.set("page", "2");
console.log(url.toString());
// https://api.ejemplo.com/v1/clientes?q=50%25+off+%23hoy&page=2
// (URLSearchParams usa + para el espacio; encodeURIComponent usa %20)
console.log(encodeURIComponent("50% off #hoy")); // 50%25%20off%20%23hoy
```

En C#:

```csharp
var q = Uri.EscapeDataString("50% off #hoy"); // "50%25%20off%20%23hoy"
var url = $"https://api.ejemplo.com/v1/clientes?q={q}&page=2";

// Leer las partes de una URL
var uri = new Uri("https://api.ejemplo.com:8443/v1/clientes/42?activo=true#datos");
Console.WriteLine($"{uri.Scheme} {uri.Host} {uri.Port} {uri.AbsolutePath} {uri.Query} {uri.Fragment}");
```

En ASP.NET Core no decodificas a mano: el model binding ya entrega `[FromQuery] string q` decodificado.

### Cuándo usarlo y cuándo no
- **Ruta** para identificar el recurso, **query** para filtrar, ordenar y paginar (ver [[Parámetros de ruta, query string y body]]).
- No pongas secretos ni datos personales en la URL: quedan en logs, historial y en el header `Referer`.
- El fragmento sirve para estado de la página en el navegador (ancla, pestaña activa), nunca para datos que el servidor necesite.
- Las URLs largas tienen límites prácticos en servidores y proxies (Pendiente de confirmar el límite exacto de cada uno); si los filtros son enormes, usa un `POST` a un recurso de búsqueda.

### Errores comunes
- **Concatenar valores sin codificar**: un `&` o `#` en el dato rompe la URL o permite inyectar parámetros. Usa `URLSearchParams`, `encodeURIComponent` o `Uri.EscapeDataString`.
- **Codificar dos veces**: `%20` se vuelve `%2520`. Codifica una sola vez, en el último paso.
- **Usar `encodeURI` para valores**: no codifica `&`, `=` ni `?`; para valores de query usa `encodeURIComponent`.
- **Olvidar que el puerto es parte del origen**: `localhost:5173` (Vite) y `localhost:5001` (API) son orígenes distintos y necesitan CORS.

### Para profundizar
- [MDN: What is a URL?](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_URL)
- [MDN: encodeURIComponent()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent)
- [RFC 3986: Uniform Resource Identifier (URI): Generic Syntax](https://www.rfc-editor.org/rfc/rfc3986)
- [WHATWG URL Standard](https://url.spec.whatwg.org/)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- ¿Qué valor recibe el servidor en el parámetro `q` de `/buscar?q=pan%20dulce%26cafe`?
> `pan dulce&cafe` (un solo parámetro: `%20` es espacio y `%26` es `&` literal).

> [!question]- Codifica para query el texto `50% off #hoy`.
> `50%25%20off%20%23hoy`. Verificable con `encodeURIComponent('50% off #hoy')`.

> [!question]- ¿Cuál es el origen (scheme + host + puerto) de `https://app.ejemplo.com/login?x=1`?
> `https://app.ejemplo.com:443` (el puerto 443 es implícito).

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- Identifica las partes de `https://ana:pw@api.ejemplo.com:8443/v1/clientes/42?activo=true#datos`.
> Esquema `https`; credenciales `ana:pw` (desaconsejadas); host `api.ejemplo.com`; puerto `8443`; ruta `/v1/clientes/42`; query `activo=true`; fragmento `datos`.

> [!question]- ¿El fragmento (`#...`) llega al servidor?
> No. El navegador lo usa localmente (anclas, routers de SPA por hash) y no lo envía en la petición HTTP.

> [!question]- ¿Por qué hay que codificar (percent-encoding) los valores de la query?
> Caracteres como espacio, `&`, `=`, `#` o `?` tienen significado en la URL. `a b&c` se envía como `a%20b%26c`. En código usa `encodeURIComponent` (JS) o `Uri.EscapeDataString` (C#), no concatenes a mano.

