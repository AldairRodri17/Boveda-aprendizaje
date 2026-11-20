---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: React
tags: [react]
---
# Componentes

Parte de: [[05-React/_Índice React|React]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un componente en React moderno es una **función de JavaScript cuyo nombre empieza con mayúscula y que devuelve JSX**. Recibe un objeto de [[Props]] como parámetro y puede tener estado propio con hooks como [[Estado con useState]]. Los componentes son la unidad de reutilización y composición: una pantalla es un árbol de componentes pequeños (un `Encabezado`, una `TablaProductos`, un `BotonGuardar`) que se anidan como etiquetas.

Los componentes de clase (`class X extends React.Component`) siguen existiendo, pero el código nuevo usa funciones; la única excepción práctica son los [[Error boundaries]].

### Cómo funciona
React trata a cada componente como una receta: cada vez que necesita saber cómo se ve, **llama a la función** con las props y el estado actuales y obtiene elementos. Eso implica una regla fundamental: el componente debe ser **puro durante el render**.

- Con las mismas props, estado y contexto, devuelve el mismo JSX.
- No modifica variables externas, no hace `fetch`, no cambia el DOM, no inicia timers mientras se ejecuta. Eso va en manejadores de eventos o en [[useEffect]].
- No muta sus props ni el estado; crea valores nuevos.

React puede llamar a tu componente más veces de las que esperas (por ejemplo, `StrictMode` lo ejecuta dos veces en desarrollo para detectar impurezas), así que cualquier efecto secundario en el cuerpo de la función se nota.

#### Por qué la mayúscula
JSX decide el tipo del elemento por la primera letra. `<boton />` se compila como la cadena `'boton'`, que React interpreta como una etiqueta HTML desconocida; `<Boton />` se compila como una referencia a la variable `Boton`, es decir, tu función. Si nombras un componente en minúscula, React crea un elemento `<boton>` vacío en el DOM y tu código nunca se ejecuta.

#### Identidad y estado
El estado de un componente vive "en la posición" que ocupa en el árbol, asociado a su **tipo**. Si en un render el tipo en esa posición cambia, React desmonta el anterior (perdiendo su estado) y monta el nuevo.

### Ejemplo
```tsx
// Producto.tsx — componente presentacional, puro
type Producto = { id: number; nombre: string; precio: number };

export function TarjetaProducto({ producto, onAgregar }: {
  producto: Producto;
  onAgregar: (id: number) => void;
}) {
  const precio = producto.precio.toLocaleString('es-MX', { style: 'currency', currency: 'MXN' });
  return (
    <div className="tarjeta">
      <h3>{producto.nombre}</h3>
      <p>{precio}</p>
      <button onClick={() => onAgregar(producto.id)}>Agregar</button>
    </div>
  );
}

// Catalogo.tsx — compone varios componentes
export function Catalogo({ productos }: { productos: Producto[] }) {
  const agregar = (id: number) => console.log('agregar', id);
  return (
    <section>
      {productos.map(p => (
        <TarjetaProducto key={p.id} producto={p} onAgregar={agregar} />
      ))}
    </section>
  );
}
```

`TarjetaProducto` no sabe de dónde vienen los productos ni qué pasa al agregar: recibe datos y avisa con un callback. Eso la hace reutilizable y fácil de probar.

### El error de declarar un componente dentro de otro
```tsx
function Formulario() {
  const [texto, setTexto] = useState('');
  // MAL: Campo es una función nueva en cada render de Formulario
  function Campo() {
    return <input value={texto} onChange={e => setTexto(e.target.value)} />;
  }
  return <Campo />;
}
```

En cada render de `Formulario` se crea una **función distinta** llamada `Campo`. Para React es un tipo de componente nuevo, así que desmonta el `<input>` anterior y monta otro: pierdes el foco con cada tecla y cualquier estado interno del hijo se reinicia. La solución es declarar `Campo` **fuera**, al nivel del módulo, y pasarle lo que necesite por props.

### Cuándo extraer un componente
- Cuando un bloque de JSX tiene un nombre claro en el dominio (`ResumenPedido`, `FiltroFechas`).
- Cuando se repite en varios lugares.
- Cuando un componente pasa de unas 150–200 líneas o mezcla responsabilidades (datos + presentación).

No hace falta extraer cada `div`: un componente demasiado fragmentado obliga a pasar muchas props (*prop drilling*). Si lo que se repite es **lógica** con estado y no marcado, lo correcto es un [[Custom hooks|custom hook]]. Para organizar muchos componentes, ver [[Estructura de carpetas por feature]].

### Errores comunes
- **Nombre en minúscula**: React lo trata como etiqueta HTML.
- **Componente declarado dentro de otro**: pierde estado y foco en cada render.
- **Efectos secundarios en el cuerpo** (fetch, `localStorage.setItem`, contadores globales): se ejecutan en cada render, y dos veces en desarrollo.
- **Mutar props** (`props.items.push(...)`): rompe la pureza y la detección de cambios.
- **Llamar al componente como función** (`{Campo()}` en vez de `<Campo />`): React no lo registra como componente y sus hooks pasan a pertenecer al padre.

### Para profundizar
- [Tu primer componente (react.dev)](https://react.dev/learn/your-first-component)
- [Mantener los componentes puros (react.dev)](https://react.dev/learn/keeping-components-pure)
- [Preservar y reiniciar el estado (react.dev)](https://react.dev/learn/preserving-and-resetting-state)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es un componente en React moderno?
> Una función cuyo nombre empieza con mayúscula y que devuelve JSX. Debe ser **pura** durante el render: con las mismas props y estado devuelve lo mismo, sin efectos secundarios.

> [!question]- ¿Por qué el nombre debe empezar con mayúscula?
> JSX trata `<boton>` como etiqueta HTML y `<Boton>` como componente.

> [!question]- Error común: declarar un componente dentro de otro. ¿Qué pasa?
> En cada render del padre se crea un tipo de componente nuevo; React desmonta y vuelve a montar el hijo, perdiendo su estado y el foco. Decláralo fuera.

## Práctica

- [[Lab 1 - App de notas con estado y efectos]]

