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
# Renderizado condicional

Parte de: [[05-React/_Índice React|React]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Renderizado condicional es mostrar u ocultar partes de la interfaz según el estado o las props: un spinner mientras carga, un mensaje de error si falló, un botón solo para administradores. Como JSX es JavaScript, no hay una sintaxis especial (`v-if`, `*ngIf`): se usan las herramientas normales del lenguaje, **`if` con `return` temprano, el ternario `? :`, el operador `&&`** y, a veces, objetos o `switch` para elegir entre varias variantes.

### Cómo funciona
React ignora en el resultado los valores `null`, `undefined`, `true` y `false`: no pintan nada. Eso permite devolver `null` para "no mostrar" o usar expresiones booleanas. Las formas principales:

#### 1. `if` con retorno temprano
Ideal para estados que cambian **toda** la salida (cargando, error, vacío):

```tsx
function ListaPedidos({ estado, pedidos }: Props) {
  if (estado === 'cargando') return <Spinner />;
  if (estado === 'error') return <Alerta tipo="error">No se pudo cargar</Alerta>;
  if (pedidos.length === 0) return <p>Sin pedidos</p>;
  return <Tabla filas={pedidos} />;
}
```

#### 2. Ternario dentro de JSX
Para elegir entre **dos** alternativas en una parte de la salida:

```tsx
<button>{guardando ? 'Guardando…' : 'Guardar'}</button>
```

#### 3. `&&` para "esto o nada"
`condicion && <X />` pinta `<X />` si la condición es verdadera. Si es falsa, el resultado es el **valor falso tal cual**. Con `false` o `null` no se pinta nada, pero con **`0`** o **`NaN`** React pinta ese número:

```tsx
{items.length && <Lista items={items} />}       // con lista vacía se ve un "0" en pantalla
{items.length > 0 && <Lista items={items} />}   // correcto: la condición es booleana
```

Regla práctica: el lado izquierdo de `&&` debe ser siempre un **booleano** (`> 0`, `!!valor`, `Boolean(valor)`).

#### 4. Mapa de variantes
Cuando hay más de dos casos, un objeto o un `switch` en una función auxiliar se lee mejor que ternarios anidados:

```tsx
// ReactNode se importa de 'react'
const iconos: Record<EstadoPedido, ReactNode> = {
  pendiente: <IconoReloj />,
  pagado: <IconoCheck />,
  cancelado: <IconoX />,
};
return <span>{iconos[pedido.estado]}</span>;
```

### Condicional contra ocultar con CSS
Hay dos formas de "esconder" algo y no son equivalentes:

| | Condicional (`{abierto && <Panel />}`) | CSS (`hidden`, `display: none`) |
|---|---|---|
| ¿Existe en el DOM? | No, se desmonta | Sí, solo no se ve |
| Estado interno del componente | Se **pierde** al ocultar | Se **conserva** |
| Efectos (`useEffect`) | Corren su limpieza al desmontar y vuelven a correr al montar | Siguen activos |
| Costo | Montar de nuevo al mostrar | Mantenerlo en memoria y renderizándose |
| Uso típico | Modales, contenido pesado que se ve poco | Pestañas que deben conservar lo escrito |

Ejemplo: un formulario en pestañas. Si cada pestaña se monta condicionalmente, al cambiar de pestaña se pierde lo que el usuario escribió (a menos que el estado viva en el padre). Si se oculta con CSS, lo escrito se conserva, pero todas las pestañas existen y se renderizan a la vez. React 19.2 agregó `<Activity mode="hidden">`, que oculta conservando el estado y pausando efectos; úsalo si tu versión lo trae.

```tsx
<div hidden={pestana !== 'datos'}><FormDatos /></div>
<div hidden={pestana !== 'domicilio'}><FormDomicilio /></div>
```

### Ejemplo completo
```tsx
type Props = { usuario: { nombre: string; esAdmin: boolean } | null; avisos: number };

export function Barra({ usuario, avisos }: Props) {
  if (!usuario) return <a href="/login">Iniciar sesión</a>;

  return (
    <nav>
      <span>Hola, {usuario.nombre}</span>
      {avisos > 0 && <span className="badge">{avisos}</span>}
      {usuario.esAdmin ? <a href="/admin">Administración</a> : null}
    </nav>
  );
}
```

### Cuándo usarlo y cuándo no
- Retorno temprano para estados globales de la pantalla; ternario para dos variantes en línea; `&&` con booleanos para "mostrar o no".
- Si encuentras ternarios anidados de tres niveles, extrae una función o un componente.
- Para estados de carga y error de datos remotos, combina con [[TanStack Query]] (`isPending`, `isError`) o con [[Suspense y lazy loading]].
- Ocultar un botón para usuarios sin permiso es **solo experiencia de usuario**: el backend debe validar el permiso (igual que en [[Rutas protegidas]]).

### Errores comunes
- **`{lista.length && ...}`** pinta `0`. Compara con `> 0`.
- **Ternarios anidados ilegibles**: usa retorno temprano o un mapa de variantes.
- **Perder estado al ocultar** sin darte cuenta: decide si el estado debe vivir en el padre o si conviene ocultar con CSS.
- **Hooks dentro de condiciones**: `if (x) { useState() }` rompe las reglas de hooks; los hooks van siempre arriba, la condición va en el JSX o después.
- **Devolver `undefined` por error** con un `if` sin `return` final en componentes antiguos: devuelve `null` explícito si no hay nada que mostrar.

### Para profundizar
- [Renderizado condicional (react.dev)](https://react.dev/learn/conditional-rendering)
- [Preservar y reiniciar el estado (react.dev)](https://react.dev/learn/preserving-and-resetting-state)
- [Activity (react.dev)](https://react.dev/reference/react/Activity)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué formas hay de renderizar condicionalmente?
> `if` con `return` temprano, ternario `cond ? <A/> : <B/>`, `cond && <A/>` y devolver `null` para no pintar nada.

> [!question]- Error común: `{items.length && <Lista />}` con lista vacía. ¿Qué se ve?
> Un `0` en pantalla: `0 && ...` devuelve `0` y React pinta números. Usa `items.length > 0 && ...`.

> [!question]- ¿Qué diferencia hay entre ocultar con condicional y con CSS?
> Con condicional el componente se desmonta (pierde estado y efectos); con CSS (`hidden`) sigue montado. Elige según si quieres conservar el estado.

## Práctica

- [[Lab 1 - App de notas con estado y efectos]]
- [[Lab 3 - Formulario controlado con validación]]

