---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: React
tags: [react]
contenido: completo
---
# JSX

Parte de: [[05-React/_Índice React|React]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
JSX es una extensión de sintaxis de JavaScript que permite escribir algo parecido a HTML dentro del código para describir la interfaz. No es HTML ni es una plantilla de texto: es **azúcar sintáctico** que el compilador (Vite con `@vitejs/plugin-react`, esbuild, SWC o TypeScript con `jsx: react-jsx`) transforma en llamadas a funciones que crean objetos de JavaScript llamados *elementos*. Por eso JSX necesita un paso de compilación y no corre directo en el navegador.

### Cómo funciona
Con la transformación moderna ("automatic runtime", la que usa React 17 en adelante), este código:

```tsx
const saludo = <h1 className="titulo">Hola, {nombre}</h1>;
```

se compila aproximadamente a:

```js
import { jsx as _jsx } from 'react/jsx-runtime';
const saludo = _jsx('h1', { className: 'titulo', children: ['Hola, ', nombre] });
```

El resultado es un objeto `{ type: 'h1', props: {...} }`. React usa esos objetos para la reconciliación (ver [[Qué es React y por qué existe]]). Consecuencias prácticas:

- **Una expresión, un valor**: un bloque JSX es una expresión, así que puedes guardarlo en variables, devolverlo de funciones o pasarlo como prop.
- **Llaves `{}` para JavaScript**: dentro de JSX, `{expresion}` evalúa cualquier *expresión* (variables, llamadas, ternarios, `map`). No admite sentencias como `if` o `for`; para eso se calcula antes del `return` o se usa un ternario (ver [[Renderizado condicional]]).
- **Mayúscula = componente**: `<boton>` se compila como la cadena `'boton'` (etiqueta HTML); `<Boton>` como la referencia a la función `Boton`.

### Diferencias de atributos con HTML
Como los atributos se convierten en propiedades de un objeto JavaScript, siguen los nombres del DOM en camelCase:

| HTML | JSX | Por qué |
|---|---|---|
| `class` | `className` | `class` es palabra reservada en JS |
| `for` | `htmlFor` | `for` es palabra reservada |
| `onclick="..."` | `onClick={fn}` | Recibe una función, no una cadena |
| `style="color: red"` | `style={{ color: 'red' }}` | Objeto con propiedades camelCase (`backgroundColor`) |
| `tabindex`, `maxlength` | `tabIndex`, `maxLength` | camelCase |
| `<input>` | `<input />` | Toda etiqueta se cierra |

Los atributos `aria-*` y `data-*` se escriben igual que en HTML (`aria-label`, `data-id`).

### Varios elementos sin un div extra
Un componente debe devolver **un solo** elemento raíz, porque una función solo devuelve un valor. Si no quieres envolver en un `div` (que puede romper un `flex`, una tabla o una lista), usa un **Fragment**:

```tsx
function Celdas({ p }: { p: Producto }) {
  // <>...</> no genera ningún nodo en el DOM
  return (
    <>
      <td>{p.nombre}</td>
      <td>{p.precio.toFixed(2)}</td>
    </>
  );
}
```

La forma corta `<>` no acepta props; si necesitas `key` (por ejemplo dentro de un `map`) usa `<Fragment key={p.id}>` importado de `react`.

### Seguridad: escape automático
Cuando insertas `{comentario}` con texto que escribió un usuario, React lo trata como **texto**: lo escapa, así que `<script>alert(1)</script>` se ve literal en pantalla y no se ejecuta. Esto protege contra XSS en el caso normal. Las puertas de riesgo son explícitas:

- `dangerouslySetInnerHTML={{ __html: html }}`: inserta HTML sin escapar. Solo con contenido sanitizado (por ejemplo con DOMPurify) o de confianza.
- URLs con `javascript:` en `href` construidas con datos del usuario: valida el esquema (`https:`), aunque React ya advierte sobre ellas.
- Atributos dinámicos de forma masiva (`{...propsDelUsuario}`) en elementos del DOM.

### Ejemplo
Una tarjeta para Itzamna.Clio que combina todo lo anterior:

```tsx
type Props = { autor: string; texto: string; destacado: boolean };

export function Comentario({ autor, texto, destacado }: Props) {
  const fecha = new Date().toLocaleDateString('es-MX');
  return (
    <article className={destacado ? 'card card--destacada' : 'card'}>
      <label htmlFor="resp">Responder a {autor}</label>
      {/* texto se escapa: es seguro aunque venga del usuario */}
      <p style={{ whiteSpace: 'pre-line' }}>{texto}</p>
      <small>{fecha}</small>
      <textarea id="resp" maxLength={500} />
    </article>
  );
}
```

### Cuándo usarlo y cuándo no
JSX es la forma estándar de escribir React; casi todo el ecosistema lo asume. Es posible usar React sin JSX (llamando a `createElement`), pero es incómodo. Si trabajas sin bundler (JavaScript ES2022 directo en el navegador), JSX no es opción: ahí encajan las plantillas con *template literals* de [[lit-html]].

### Errores comunes
- **Usar `class` o `for`**: funciona con advertencia en consola o no hace lo esperado; usa `className` y `htmlFor`.
- **Poner `if` dentro de las llaves**: es una sentencia, no una expresión. Calcula antes o usa ternario.
- **Devolver dos hermanos sin Fragment**: error de compilación ("Adjacent JSX elements must be wrapped").
- **Escribir `{0 && <X />}`**: React pinta el `0`. Ver [[Renderizado condicional]].
- **Usar `dangerouslySetInnerHTML` para "formatear"**: casi siempre hay alternativa con componentes.

### Para profundizar
- [Escribir marcado con JSX (react.dev)](https://react.dev/learn/writing-markup-with-jsx)
- [JavaScript en JSX con llaves (react.dev)](https://react.dev/learn/javascript-in-jsx-with-curly-braces)
- [Componentes comunes del DOM y sus props (react.dev)](https://react.dev/reference/react-dom/components/common)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es JSX y en qué se convierte?
> Una sintaxis tipo HTML dentro de JS que el compilador (Babel/esbuild vía Vite) transforma en llamadas que crean elementos de React (`jsx('div', props)`). No es HTML: es JS.

> [!question]- ¿Qué diferencias de atributos hay con HTML?
> `className` en vez de `class`, `htmlFor` en vez de `for`, eventos en camelCase (`onClick`), `style` recibe un objeto (`style={{ marginTop: 8 }}`) y todas las etiquetas deben cerrarse (`<img />`).

> [!question]- ¿Cómo devuelves varios elementos sin un `div` extra?
> Con un Fragment: `<>...</>` (o `<Fragment key=...>` si necesitas `key` en una lista).

> [!question]- ¿Es seguro `{comentario}` con texto del usuario?
> Sí: React escapa el texto. El riesgo está en `dangerouslySetInnerHTML` y en `href={url}` con URLs `javascript:`.

