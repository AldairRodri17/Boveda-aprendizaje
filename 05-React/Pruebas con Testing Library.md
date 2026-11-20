---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: React
tags: [react]
---
# Pruebas con Testing Library

Parte de: [[05-React/_Índice React|React]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
React Testing Library (paquete `@testing-library/react`) es una biblioteca para probar componentes **como los usa una persona**: renderizas el componente en un DOM simulado (jsdom o happy-dom), lo buscas por lo que el usuario ve —su rol, su etiqueta, su texto— e interactúas con él (clic, escribir) para afirmar el resultado visible. No es un *test runner*: se usa junto con **Vitest** (lo natural con Vite) o Jest, que ejecutan las pruebas y dan `describe`, `it`, `expect`. Suele acompañarse de `@testing-library/user-event` para interacciones y `@testing-library/jest-dom` para afirmaciones como `toBeInTheDocument()`.

### La filosofía
Su principio guía es: *cuanto más se parezcan tus pruebas a la forma en que se usa el software, más confianza te dan*. En la práctica:

- **No pruebes detalles de implementación**: ni el estado interno, ni los nombres de clases CSS, ni cuántas veces se llamó un hook. Si refactorizas (de `useState` a `useReducer`, de CSS Modules a Tailwind) y el usuario ve lo mismo, la prueba debe seguir pasando.
- **Busca como busca una persona**, en este orden de preferencia: `getByRole` (con `name`), `getByLabelText`, `getByPlaceholderText`, `getByText`, `getByDisplayValue`; y como último recurso `getByTestId`. Buscar por rol además te obliga a escribir HTML accesible: si no encuentras el botón por su rol y nombre, un lector de pantalla tampoco.

Es la misma idea que probar una API ASP.NET Core con `WebApplicationFactory` haciendo peticiones HTTP reales, en lugar de probar métodos privados del controlador.

### Cómo funciona: tipos de consultas
| Prefijo | Si no encuentra | Si encuentra varios | Uso |
|---|---|---|---|
| `getBy…` | Lanza error | Lanza error | Afirmar que algo **está** ahora |
| `queryBy…` | Devuelve `null` | Lanza error | Afirmar que algo **no está**: `expect(queryByText('Error')).not.toBeInTheDocument()` |
| `findBy…` | Promesa que se rechaza tras el tiempo de espera (1 s por defecto) | Rechaza | Esperar algo que **aparecerá** (después de un fetch o un efecto) |
| `getAllBy…`, `queryAllBy…`, `findAllBy…` | Igual, pero devuelven arreglos | — | Listas |

Para afirmaciones asíncronas que no son "aparece un elemento", existe `waitFor(() => expect(...))`.

#### userEvent contra fireEvent
`fireEvent.click(boton)` dispara **un solo** evento DOM sintético. `userEvent` simula la **secuencia completa** que haría un navegador: al escribir, enfoca el input, dispara `keydown`, `keypress`, `input`, `keyup` por cada tecla; al hacer clic, mueve el puntero, dispara `pointerdown`, `mousedown`, `focus`, `pointerup`, `mouseup`, `click`. Además respeta reglas reales: no puedes escribir en un input `disabled` ni hacer clic en un elemento con `pointer-events: none`. Con la versión 14 se usa así: `const user = userEvent.setup()` y luego `await user.click(...)` (todas las acciones son asíncronas).

### Ejemplo con Vitest
Componente: un formulario que valida y llama a `onGuardar`.

```tsx
// FormContacto.tsx
export function FormContacto({ onGuardar }: { onGuardar: (correo: string) => Promise<void> }) {
  const [correo, setCorreo] = useState('');
  const [error, setError] = useState('');
  const [ok, setOk] = useState(false);

  async function enviar(e: FormEvent) {
    e.preventDefault();
    if (!correo.includes('@')) { setError('Correo inválido'); return; }
    setError('');
    await onGuardar(correo);
    setOk(true);
  }

  return (
    <form onSubmit={enviar}>
      <label>Correo <input value={correo} onChange={e => setCorreo(e.target.value)} /></label>
      {error && <p role="alert">{error}</p>}
      {ok && <p>Guardado</p>}
      <button type="submit">Guardar</button>
    </form>
  );
}
```

```tsx
// FormContacto.test.tsx
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { describe, expect, it, vi } from 'vitest';
import { FormContacto } from './FormContacto';

describe('FormContacto', () => {
  it('muestra error con correo inválido y no guarda', async () => {
    const user = userEvent.setup();
    const onGuardar = vi.fn();
    render(<FormContacto onGuardar={onGuardar} />);

    await user.type(screen.getByLabelText('Correo'), 'sin-arroba');
    await user.click(screen.getByRole('button', { name: 'Guardar' }));

    expect(screen.getByRole('alert')).toHaveTextContent('Correo inválido');
    expect(onGuardar).not.toHaveBeenCalled();
  });

  it('guarda y confirma con correo válido', async () => {
    const user = userEvent.setup();
    const onGuardar = vi.fn().mockResolvedValue(undefined);
    render(<FormContacto onGuardar={onGuardar} />);

    await user.type(screen.getByLabelText('Correo'), 'ana@xolotl.mx');
    await user.click(screen.getByRole('button', { name: 'Guardar' }));

    expect(await screen.findByText('Guardado')).toBeInTheDocument();
    expect(onGuardar).toHaveBeenCalledWith('ana@xolotl.mx');
    expect(screen.queryByRole('alert')).not.toBeInTheDocument();
  });
});
```

Configuración mínima en Vite: en `vite.config.ts`, `test: { environment: 'jsdom', setupFiles: './src/test/setup.ts' }`, y en el setup `import '@testing-library/jest-dom/vitest'`. Para componentes que consultan la API, envuelve con un `QueryClientProvider` nuevo por prueba y simula la red con **MSW** (Mock Service Worker) en lugar de simular `fetch` a mano. En Azure DevOps, `vitest run` con el reporter `junit` publica los resultados en el pipeline.

### Cuándo usarlo y cuándo no
Úsalo para pruebas de **componentes y flujos de una pantalla**: formularios, validación, estados de carga y error, permisos que ocultan botones. La lógica pura (reducers, formateadores) se prueba más rápido como funciones normales. Los flujos completos entre pantallas, con backend real y navegador real, van mejor con pruebas end-to-end (Playwright).

### Errores comunes
- **Buscar por clase o `data-testid` por costumbre**: pruebas frágiles y sin beneficio de accesibilidad.
- **Usar `getBy` para afirmar ausencia**: lanza error; usa `queryBy`.
- **No esperar lo asíncrono**: usar `getBy` justo después de un fetch; usa `findBy`.
- **Olvidar `await` en `user.*`**: la acción no termina antes de la afirmación.
- **Envolver todo en `act()` a mano**: Testing Library ya lo hace; si ves advertencias de `act`, normalmente falta esperar algo con `findBy` o `waitFor`.

### Para profundizar
- [Introducción a React Testing Library](https://testing-library.com/docs/react-testing-library/intro/)
- [Prioridad de consultas (Testing Library)](https://testing-library.com/docs/queries/about/#priority)
- [user-event (Testing Library)](https://testing-library.com/docs/user-event/intro)
- [Vitest](https://vitest.dev/guide/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cuál es la filosofía de Testing Library?
> Probar como lo usa una persona: buscar elementos por rol y texto (`getByRole('button', { name: 'Guardar' })`), no por clases ni detalles internos del componente.

> [!question]- ¿Qué diferencia hay entre `getBy`, `queryBy` y `findBy`?
> `getBy`: lanza si no existe. `queryBy`: devuelve `null` (para afirmar que algo NO está). `findBy`: promesa que espera a que aparezca (asíncrono).

> [!question]- ¿Por qué `userEvent` en vez de `fireEvent`?
> `userEvent` simula la secuencia real (foco, teclas, input, clic) y detecta cosas como elementos deshabilitados; `fireEvent` dispara un solo evento DOM.

## Práctica

- [[Lab 3 - Formulario controlado con validación]]

