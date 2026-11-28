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
# Context

Parte de: [[05-React/_Índice React|React]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Context es el mecanismo de React para **pasar datos a todo un subárbol de componentes sin enviarlos prop por prop**. Un componente ancestro "provee" un valor y cualquier descendiente, a cualquier profundidad, lo "consume" con `useContext` (o con `use` en React 19). Resuelve el *prop drilling* (ver [[Props]]) para datos que muchos componentes necesitan: la sesión del usuario, el tema visual, el idioma, permisos o la configuración de una sección.

### Cómo funciona
1. **Crear** el contexto con un valor por defecto: `const SesionContext = createContext<Sesion | null>(null)`.
2. **Proveer** el valor envolviendo un subárbol: `<SesionContext value={sesion}>…</SesionContext>` (en React 19 el propio contexto funciona como proveedor; en versiones anteriores se usa `SesionContext.Provider`).
3. **Consumir** en cualquier descendiente: `const sesion = useContext(SesionContext)`. React busca hacia arriba el proveedor más cercano; si no hay, usa el valor por defecto.
4. Cuando el `value` del proveedor **cambia** (por referencia, `Object.is`), React re-renderiza **todos** los componentes que consumen ese contexto, aunque usen solo una parte del valor.

```mermaid
flowchart TD
    P["SesionProvider (estado: usuario)"] --> L["Layout (no consume)"]
    L --> B["Barra"] --> A["Avatar: useSesion()"]
    L --> M["Main"] --> F["Formulario: useSesion()"]
    P -. "value cambia → re-render de consumidores" .-> A
    P -. .-> F
```

### Ejemplo: sesión para la app
El patrón recomendado: un archivo con el contexto, un componente proveedor y un custom hook que valida que se use dentro del proveedor.

```tsx
// features/auth/SesionContext.tsx
import { createContext, useContext, useMemo, useState, type ReactNode } from 'react';

type Usuario = { id: string; nombre: string; roles: string[] };
type ValorSesion = {
  usuario: Usuario | null;
  iniciar: (u: Usuario) => void;
  cerrar: () => void;
};

const SesionContext = createContext<ValorSesion | null>(null);

export function SesionProvider({ children }: { children: ReactNode }) {
  const [usuario, setUsuario] = useState<Usuario | null>(null);

  // useMemo: el objeto solo cambia cuando cambia usuario
  const valor = useMemo<ValorSesion>(() => ({
    usuario,
    iniciar: setUsuario,
    cerrar: () => setUsuario(null),
  }), [usuario]);

  return <SesionContext value={valor}>{children}</SesionContext>;
}

export function useSesion() {
  const ctx = useContext(SesionContext);
  if (!ctx) throw new Error('useSesion debe usarse dentro de <SesionProvider>');
  return ctx;
}
```

```tsx
// Cualquier componente profundo
function Avatar() {
  const { usuario, cerrar } = useSesion();
  return usuario ? <button onClick={cerrar}>{usuario.nombre} · Salir</button> : null;
}
```

Esto es lo que usarían las [[Rutas protegidas]] para decidir si hay sesión. Ojo: en apps con tokens como las que consumen a Xolotl.Hecate, el token real vive en memoria o en una cookie `HttpOnly`, no en `localStorage`; el contexto solo expone el estado que la UI necesita.

### El problema de rendimiento
Context no tiene selectores: **todo consumidor se re-renderiza cuando cambia el valor**, aunque solo lea un campo que no cambió. Dos fuentes típicas de renders de más:

1. **Valor nuevo en cada render del proveedor**: `<Ctx value={{ usuario, cerrar }}>` crea un objeto nuevo cada vez que el proveedor se renderiza, aunque `usuario` sea el mismo. Solución: `useMemo` del valor (como en el ejemplo) o pasar solo valores primitivos/estables. Con el React Compiler esto se memoriza solo.
2. **Contexto "dios"** con todo adentro (sesión, tema, carrito, filtros): cualquier cambio re-renderiza a todos. Solución: **dividir** en varios contextos por responsabilidad y frecuencia de cambio (por ejemplo, uno para datos y otro para acciones, que casi nunca cambian).

Para valores que cambian poco (sesión, tema, idioma) esto no importa. Para valores que cambian muchas veces por segundo (posición del mouse, texto que se escribe, un carrito muy activo) se nota.

### ¿Context reemplaza a un manejador de estado?
No exactamente. Context es un mecanismo de **transporte** (inyección de dependencias para el árbol), no un manejador de estado: el estado sigue viviendo en un `useState` o `useReducer` del proveedor. Combinado con [[useReducer]] cubre bien estado compartido moderado. Pero no ofrece suscripción selectiva, ni herramientas de depuración, ni persistencia, ni acceso fuera de React. Cuando el estado de cliente es grande o cambia seguido, un store como [[zustand]] (con selectores que re-renderizan solo a quien usa el campo que cambió) escala mejor. Y para **datos del servidor**, ni Context ni zustand: [[TanStack Query]].

| Necesidad | Herramienta |
|---|---|
| Sesión, tema, idioma, configuración | Context |
| Estado de cliente compartido y frecuente | zustand |
| Datos de la API (caché, reintentos) | TanStack Query |
| Estado local de un componente | useState / useReducer |

### Errores comunes
- **Olvidar el proveedor**: el consumidor recibe el valor por defecto (`null`) y falla lejos del origen. El custom hook con `throw` lo detecta temprano.
- **Valor sin memoizar**: re-renders de todos los consumidores en cada render del proveedor.
- **Un solo contexto para todo**: divide por responsabilidad.
- **Usar Context para evitar pasar una prop un nivel**: las props explícitas son más claras.
- **Guardar datos del servidor en Context** y re-implementar caché a mano.

### Para profundizar
- [Pasar datos en profundidad con contexto (react.dev)](https://react.dev/learn/passing-data-deeply-with-context)
- [useContext (react.dev)](https://react.dev/reference/react/useContext)
- [createContext (react.dev)](https://react.dev/reference/react/createContext)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Para qué sirve Context?
> Para que un valor llegue a cualquier descendiente sin pasarlo por props: tema, idioma, usuario autenticado. Se crea con `createContext`, se provee con `<Ctx value={...}>` (o `<Ctx.Provider>`) y se lee con `useContext(Ctx)`.

> [!question]- ¿Cuál es su problema de rendimiento?
> Cuando el valor cambia, **todos** los consumidores se re-renderizan. Si el valor es un objeto creado en cada render, cambia siempre. Memoriza el valor, divide en varios contextos, o usa un store con selectores (zustand).

> [!question]- ¿Context reemplaza a un manejador de estado?
> Es un mecanismo de transporte, no de manejo: no trae selectores ni caché. Para datos del servidor usa TanStack Query; para estado global de UI que cambia seguido, un store.

## Práctica

- [[Lab 4 - Rutas protegidas con React Router (data router)]]

