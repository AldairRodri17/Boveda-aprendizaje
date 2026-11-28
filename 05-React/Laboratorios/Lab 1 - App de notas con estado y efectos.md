---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: React
herramienta: Vite + React, VS Code y React DevTools
tags: [react, laboratorio]
contenido: completo
---
# Lab 1 - App de notas con estado y efectos

Parte de: [[05-React/_Índice React|React]]

**Cubre:** [[Componentes]] · [[Props]] · [[Estado con useState]] · [[Eventos en React]] · [[Listas y keys]] · [[Renderizado condicional]] · [[useEffect]] · [[useRef]] · [[Custom hooks]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Construir una app pequeña de notas con alta, borrado, filtro y persistencia en `localStorage`, usando estado, efectos con limpieza y un custom hook, y verificar los renders con React DevTools.

## Pasos

1. Crea el proyecto con `npm create vite@latest <nombre> -- --template react` (o `react-ts`), luego `npm install` y `npm run dev`. Guarda el proyecto en esta carpeta.
2. Componentes: `App`, `FormNota` (input + botón), `ListaNotas` y `Nota` (texto y botón borrar). Pasa datos por props y callbacks (`onAgregar`, `onBorrar`).
3. Estado en `App`: `notas` (arreglo de `{ id, texto, creada }`) y `filtro` (texto). Agrega notas sin mutar el arreglo y usa `crypto.randomUUID()` como `key`.
4. La lista muestra solo las notas que contienen el filtro (calculado en el render, no en un efecto). Si no hay notas, muestra "Sin notas".
5. Crea `useLocalStorage(clave, inicial)` que lea al iniciar (con `useState(() => ...)`) y guarde en un `useEffect` cuando cambie el valor.
6. Agrega un reloj "Hace X segundos" en cada nota con un `setInterval` en un efecto que se limpie al desmontar.
7. Enfoca el input al cargar la app y después de agregar con `useRef`.
8. Abre React DevTools → Profiler, graba mientras escribes en el filtro y observa qué componentes se renderizan.

## Criterios de terminado

- [ ] Agregar, filtrar y borrar funcionan; con la lista vacía aparece "Sin notas" (y nunca un `0` suelto).
- [ ] Al recargar, las notas siguen ahí.
- [ ] Al borrar una nota, su intervalo se limpia (no hay avisos en consola ni intervalos huérfanos: compruébalo con un `console.log` en la limpieza).
- [ ] No hay avisos de `key` en consola.
- [ ] El input queda enfocado tras agregar.

## Solución

> [!success]- useLocalStorage
> ```jsx
> function useLocalStorage(clave, inicial) {
>   const [valor, setValor] = useState(() => {
>     try { return JSON.parse(localStorage.getItem(clave)) ?? inicial; } catch { return inicial; }
>   });
>   useEffect(() => { localStorage.setItem(clave, JSON.stringify(valor)); }, [clave, valor]);
>   return [valor, setValor];
> }
> ```

> [!success]- App y reloj por nota
> ```jsx
> export default function App() {
>   const [notas, setNotas] = useLocalStorage('notas', []);
>   const [filtro, setFiltro] = useState('');
>   const inputRef = useRef(null);
>   useEffect(() => { inputRef.current?.focus(); }, []);
>
>   const visibles = notas.filter(n => n.texto.toLowerCase().includes(filtro.toLowerCase()));
>   const agregar = (texto) => {
>     setNotas(ns => [...ns, { id: crypto.randomUUID(), texto, creada: Date.now() }]);
>     inputRef.current?.focus();
>   };
>   const borrar = (id) => setNotas(ns => ns.filter(n => n.id !== id));
>
>   return (
>     <main>
>       <FormNota onAgregar={agregar} inputRef={inputRef} />
>       <input placeholder="Filtrar" value={filtro} onChange={e => setFiltro(e.target.value)} />
>       {visibles.length > 0
>         ? <ul>{visibles.map(n => <Nota key={n.id} nota={n} onBorrar={borrar} />)}</ul>
>         : <p>Sin notas</p>}
>     </main>
>   );
> }
>
> function Nota({ nota, onBorrar }) {
>   const [ahora, setAhora] = useState(Date.now());
>   useEffect(() => {
>     const id = setInterval(() => setAhora(Date.now()), 1000);
>     return () => clearInterval(id);
>   }, []);
>   const seg = Math.round((ahora - nota.creada) / 1000);
>   return <li>{nota.texto} <small>hace {seg} s</small> <button onClick={() => onBorrar(nota.id)}>Borrar</button></li>;
> }
> ```

## Notas personales

_Qué me costó, errores que cometí, dudas._
