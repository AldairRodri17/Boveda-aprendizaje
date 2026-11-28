---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: React
tags: [react]
contenido: completo
---
# useRef

Parte de: [[05-React/_Índice React|React]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`useRef` crea una **caja mutable** que React conserva entre renders: `const ref = useRef(valorInicial)` devuelve un objeto `{ current: valorInicial }` que es **el mismo objeto** durante toda la vida del componente. Puedes leer y escribir `ref.current` libremente, y **cambiarlo no provoca un nuevo render**. Tiene dos usos principales: obtener una referencia a un **nodo del DOM** (para enfocar un input, medir un elemento, integrar una librería imperativa) y guardar **valores que no afectan lo que se ve** (el id de un intervalo, un temporizador, el valor previo de una prop, una instancia de un objeto externo).

### Cómo funciona
- En el primer render React crea el objeto `{ current: inicial }`; en los siguientes devuelve el mismo objeto.
- Si pasas el ref como atributo `ref` a un elemento JSX (`<input ref={inputRef} />`), React asigna el nodo del DOM a `inputRef.current` durante el **commit**, y lo vuelve a `null` cuando el elemento se desmonta.
- Como React no "vigila" `current`, cambiarlo no actualiza la pantalla. Si quieres que la UI refleje el cambio, eso es estado, no ref.
- En React 19 los componentes de función pueden recibir `ref` **como una prop normal**, sin `forwardRef`. Con eso un componente `Campo` puede exponer su `<input>` interno al padre.

### Estado o ref: cómo decidir
| Pregunta | Estado (`useState`) | Ref (`useRef`) |
|---|---|---|
| ¿Cambiarlo debe actualizar la pantalla? | Sí | No |
| ¿Se lee durante el render para decidir qué mostrar? | Sí | No (solo en handlers y efectos) |
| ¿Se actualiza de inmediato? | No, en el siguiente render | Sí, `current` cambia en el acto |
| Ejemplos | Texto de un input, modal abierto, lista | Nodo DOM, id de `setInterval`, `AbortController`, contador de reintentos |

Regla corta: si lo muestras, es estado; si solo lo necesitas "tras bambalinas", es ref.

### Ejemplos
#### Enfocar un input y medir un elemento
```tsx
import { useEffect, useRef } from 'react';

export function Buscador() {
  const inputRef = useRef<HTMLInputElement>(null);

  useEffect(() => {
    inputRef.current?.focus(); // el nodo ya existe: los efectos corren tras el commit
  }, []);

  function limpiar() {
    if (inputRef.current) {
      inputRef.current.value = '';
      inputRef.current.focus();
    }
  }

  return (
    <div>
      <input ref={inputRef} placeholder="Buscar cliente" />
      <button type="button" onClick={limpiar}>Limpiar</button>
    </div>
  );
}
```

(En un input controlado limpiarías el estado; aquí el input es no controlado a propósito para mostrar acceso directo al DOM.)

#### Guardar el id de un temporizador
```tsx
export function Cronometro() {
  const [segundos, setSegundos] = useState(0);
  const intervalo = useRef<number | null>(null); // no se muestra: es ref

  function iniciar() {
    if (intervalo.current !== null) return;      // ya corre
    intervalo.current = window.setInterval(() => setSegundos(s => s + 1), 1000);
  }

  function detener() {
    if (intervalo.current !== null) window.clearInterval(intervalo.current);
    intervalo.current = null;
  }

  useEffect(() => detener, []); // limpia al desmontar

  return (
    <p>
      {segundos}s <button onClick={iniciar}>Iniciar</button> <button onClick={detener}>Detener</button>
    </p>
  );
}
```

#### Exponer un input interno (React 19)
```tsx
function Campo({ etiqueta, ref }: { etiqueta: string; ref?: React.Ref<HTMLInputElement> }) {
  return <label>{etiqueta} <input ref={ref} /></label>;
}
```

### Por qué no leer ni escribir `ref.current` durante el render
El render debe ser **puro**: el mismo input produce el mismo JSX. Un ref es mutable y React no sabe cuándo cambia, así que:

- **Leerlo en el render** hace que la salida dependa de algo que React no rastrea: la pantalla puede quedar desactualizada porque cambiar el ref no provoca render. Además, durante el render los refs del DOM todavía pueden ser `null` (el nodo se asigna en el commit).
- **Escribirlo en el render** es un efecto secundario: React puede renderizar varias veces sin hacer commit (modo concurrente, StrictMode) y tu valor quedaría inconsistente.

La excepción aceptada es la inicialización perezosa: `if (ref.current === null) ref.current = new ObjetoCaro();`. Fuera de eso, lee y escribe refs en **manejadores de eventos y efectos**.

### Cuándo usarlo y cuándo no
Úsalo para manejar foco, selección de texto, scroll, medición (`getBoundingClientRect`), reproducir video, integrar librerías imperativas (gráficas, mapas, editores, el recortador de [[react-easy-crop]] cuando necesitas el canvas) o guardar valores mutables que no se pintan. No lo uses para esquivar re-renders cuando el valor **sí** debe verse, ni para manipular el DOM que React administra (agregar o quitar hijos a mano): eso rompe la reconciliación. Para valores derivados costosos usa [[useMemo y useCallback]].

### Errores comunes
- **Usar ref para algo que se muestra**: la pantalla no se actualiza.
- **Leer `ref.current` en el render**: datos viejos o `null`.
- **Olvidar el `?.`** al acceder al nodo: puede ser `null` antes del montaje o después del desmontaje.
- **Modificar el DOM administrado por React** vía ref: conflictos con la reconciliación.
- **Tipar mal**: `useRef<HTMLInputElement>(null)` para nodos; `useRef<number | null>(null)` para ids.

### Para profundizar
- [useRef (react.dev)](https://react.dev/reference/react/useRef)
- [Referenciar valores con refs (react.dev)](https://react.dev/learn/referencing-values-with-refs)
- [Manipular el DOM con refs (react.dev)](https://react.dev/learn/manipulating-the-dom-with-refs)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Para qué sirve `useRef`?
> Guarda un valor mutable que persiste entre renders **sin provocar re-render** al cambiarlo (`ref.current`). Usos: referencia a un nodo del DOM, ids de timers, valor previo, flags.

> [!question]- ¿Cuándo usar estado y cuándo ref?
> Si el valor se muestra en pantalla, estado. Si solo lo usan handlers o efectos y no afecta lo que se pinta, ref.

> [!question]- Error común: leer o escribir `ref.current` durante el render. ¿Por qué evitarlo?
> Hace el render impuro e impredecible (React puede renderizar varias veces). Úsalo en handlers y efectos.

## Práctica

- [[Lab 1 - App de notas con estado y efectos]]

