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
# Template literals

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Los **template literals** (plantillas de cadena) son cadenas delimitadas con **comillas invertidas** `` ` `` en lugar de comillas simples o dobles. Permiten **interpolar** expresiones con `${...}`, escribir **varias líneas** sin `\n` y, en su forma "etiquetada" (*tagged*), procesar la plantilla con una función.

### Cómo funciona
```js
const cliente = 'Ana';
const total = 1250.5;

// Interpolación: cualquier expresión, se convierte a cadena
const msg = `Hola, ${cliente}. Tu total es ${total.toFixed(2)} (${total > 1000 ? 'mayoreo' : 'menudeo'})`;

// Multilínea: los saltos y espacios se conservan tal cual
const sql = `
  SELECT Id, Total
  FROM Pedidos
  WHERE ClienteId = @ClienteId`;
```

Frente a las comillas normales, ganas:
- **Interpolación** legible en vez de concatenar con `+`.
- **Varias líneas** literales.
- **Comillas simples y dobles** dentro sin escapar.
- **Plantillas etiquetadas**.

Cada `${}` se convierte con las reglas de cadena (ver [[Coerción de tipos]]): `${null}` produce `"null"`, `${[1, 2]}` produce `"1,2"` y un objeto produce `"[object Object]"`.

### Tagged templates (plantillas etiquetadas)
Si pones una función justo antes de la plantilla, JS la llama con **los fragmentos de texto** en un arreglo y **los valores interpolados** como argumentos aparte. La función decide qué devolver (no tiene que ser una cadena):

```js
function resaltar(textos, ...valores) {
  // textos: ['Pedido ', ' de ', ''] ; valores: [1024, 'Ana']
  return textos.reduce((acc, t, i) => acc + t + (i < valores.length ? `<b>${valores[i]}</b>` : ''), '');
}
resaltar`Pedido ${1024} de ${'Ana'}`; // 'Pedido <b>1024</b> de <b>Ana</b>'
```

Usos reales:
- **`String.raw`**: devuelve la cadena sin procesar escapes: `` String.raw`C:\temp\nuevo` `` conserva las barras.
- **Escapar HTML** automáticamente en cada valor (como hace [[lit-html]], cuyo `` html`...` `` crea plantillas seguras y eficientes).
- **Consultas parametrizadas**: bibliotecas de SQL que convierten cada `${}` en un parámetro en lugar de pegarlo en el texto.
- **Estilos** en algunas bibliotecas CSS-in-JS.

### El riesgo de seguridad: `innerHTML`
```js
el.innerHTML = `<p>${comentario}</p>`; // peligroso
```

Si `comentario` viene del usuario o de la API, puede contener HTML: `<img src=x onerror="robarToken()">`. Al asignarlo a `innerHTML`, el navegador lo interpreta y ejecuta el script: es **XSS** (*cross-site scripting*). Las plantillas no escapan nada por sí solas; solo pegan texto.

Alternativas seguras:

```js
// 1. Crear el elemento y usar textContent (nunca interpreta HTML)
const p = document.createElement('p');
p.textContent = comentario;
el.replaceChildren(p);

// 2. Escapar cada valor con una plantilla etiquetada
const ESC = { '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;', "'": '&#39;' };
const escapar = (v) => String(v).replace(/[&<>"']/g, (c) => ESC[c]);
const html = (textos, ...vals) => textos.reduce((a, t, i) => a + t + (i < vals.length ? escapar(vals[i]) : ''), '');
el.innerHTML = html`<p>${comentario}</p>`;

// 3. Usar lit-html o React, que escapan por defecto
```

Más detalle en [[Seguridad del DOM (innerHTML y escape de datos)]].

### Ejemplo
Construir una URL de la API con parámetros seguros y un mensaje para el usuario:

```js
const qs = new URLSearchParams({ cliente: '7', estado: 'nuevo' });
const url = `/api/pedidos?${qs}`;          // URLSearchParams codifica los valores

const resumen = `${pedidos.length} pedido${pedidos.length === 1 ? '' : 's'} encontrados`;
```

No armes URLs interpolando valores sin codificar (`` `/api/buscar?q=${texto}` ``): un `&` o `#` en el texto rompe la URL. Usa `URLSearchParams` o `encodeURIComponent`.

### Detalles de escape
Dentro de una plantilla, los escapes normales siguen funcionando (`\n`, `\t`, `é`). Para escribir una comilla invertida literal usa `` \` `` y para un `${` literal, `\${`. Si necesitas el texto tal cual, sin interpretar escapes (rutas de Windows, expresiones regulares), usa `String.raw`. En las plantillas etiquetadas, la función recibe también `textos.raw`, un arreglo con los fragmentos sin procesar, que es lo que usa `String.raw` por dentro. Por último, el resultado de una plantilla sin etiqueta **siempre es una cadena nueva**: se evalúa cada vez que se ejecuta la línea, así que puede contener cualquier expresión, incluidas llamadas a funciones, ternarios y otras plantillas anidadas.

### Cuándo usarlo y cuándo no
Úsalos para cualquier cadena con valores dinámicos o varias líneas. No los uses para meter datos no confiables en HTML, SQL o URLs sin escapar o parametrizar. Para traducir textos con plurales y formatos, combina con `Intl` (ver [[Intl (formato de números y fechas)]]).

### Errores comunes
- **Usar comillas normales** con `${}`: `'Hola ${nombre}'` imprime el texto literal.
- **XSS** con `innerHTML` y plantillas.
- **Espacios y saltos no deseados** en plantillas multilínea (la sangría del código queda en la cadena).
- **Interpolar objetos** y obtener `[object Object]`.

### Para profundizar
- [Template literals (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)
- [`String.raw()` (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/raw)
- [Prevención de XSS (OWASP Cheat Sheet)](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- ¿Qué imprime este código?
> ```js
> function tag(s, ...v) { return s.raw.join('|') + '::' + v.join(','); }
> console.log(tag`a${1}b${2}c`);
> ```
>
> > [!success]- Respuesta
> > `a|b|c::1,2`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué permiten los template literals frente a las comillas normales?
> Interpolar expresiones con `${}`, escribir varias líneas sin `\n` y crear *tagged templates*.

> [!question]- ¿Qué es un tagged template?
> Una función que recibe las partes fijas del texto y los valores por separado: ``html`<p>${nombre}</p>` ``. Así lo usan lit-html (para saber qué partes son dinámicas) o librerías de escape/SQL.

> [!question]- Error de seguridad: ``el.innerHTML = `<p>${comentario}</p>` ``. ¿Qué riesgo hay?
> XSS: si `comentario` trae HTML/JS del usuario, se interpreta. Interpolar no escapa nada; usa `textContent` o una plantilla que escape (lit-html).

