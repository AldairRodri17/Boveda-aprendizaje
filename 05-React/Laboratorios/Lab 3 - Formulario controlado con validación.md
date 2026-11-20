---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: React
herramienta: Vite + React y Vitest + Testing Library
tags: [react, laboratorio]
---
# Lab 3 - Formulario controlado con validación

Parte de: [[05-React/_Índice React|React]]

**Cubre:** [[Componentes controlados]] · [[useReducer]] · [[Eventos en React]] · [[Renderizado condicional]] · [[Pruebas con Testing Library]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Hacer un formulario de registro controlado con validación por campo (al salir del campo y al enviar), mensajes accesibles, botón deshabilitado mientras se envía y pruebas con Testing Library.

## Pasos

1. Crea el proyecto con `npm create vite@latest <nombre> -- --template react` (o `react-ts`), luego `npm install` y `npm run dev`. Instala `vitest`, `jsdom`, `@testing-library/react`, `@testing-library/user-event` y `@testing-library/jest-dom`.
2. Campos: nombre (requerido), correo (formato), contraseña (mín. 8 con un número) y confirmación (igual a la contraseña).
3. Maneja el estado con `useReducer`: `valores`, `tocados` y `enviando`. Escribe `validar(valores)` como función pura que devuelve un objeto de errores.
4. Muestra el error de un campo solo si fue tocado (`onBlur`) o si se intentó enviar. Asócialo con `aria-describedby` y marca `aria-invalid`.
5. Al enviar con errores, enfoca el primer campo inválido. Sin errores, simula el envío con una promesa de 1 s y deshabilita el botón mientras tanto.
6. Escribe pruebas: (a) enviar vacío muestra 4 errores, (b) contraseñas distintas muestra el error de confirmación, (c) datos válidos deshabilitan el botón y luego muestran "Cuenta creada".
7. Corre `npx vitest`.

## Criterios de terminado

- [ ] Escribir en un campo no muestra su error hasta salir de él.
- [ ] Enviar vacío muestra los errores y enfoca el primer campo inválido.
- [ ] Mientras "envía", el botón está deshabilitado y no se puede enviar dos veces.
- [ ] `validar` tiene pruebas unitarias propias (sin renderizar).
- [ ] Las 3 pruebas de componente pasan y buscan elementos por rol o etiqueta, no por clase.

## Solución

> [!success]- Reducer y validación
> ```jsx
> const inicial = { valores: { nombre: '', correo: '', pass: '', pass2: '' }, tocados: {}, enviando: false, intento: false };
> function reducer(s, a) {
>   switch (a.type) {
>     case 'cambiar': return { ...s, valores: { ...s.valores, [a.campo]: a.valor } };
>     case 'tocar': return { ...s, tocados: { ...s.tocados, [a.campo]: true } };
>     case 'intentar': return { ...s, intento: true };
>     case 'enviando': return { ...s, enviando: a.valor };
>     default: throw new Error(a.type);
>   }
> }
> export function validar(v) {
>   const e = {};
>   if (!v.nombre.trim()) e.nombre = 'Escribe tu nombre';
>   if (!/^[^@\s]+@[^@\s]+\.[^@\s]+$/.test(v.correo)) e.correo = 'Correo no válido';
>   if (v.pass.length < 8 || !/\d/.test(v.pass)) e.pass = 'Mínimo 8 caracteres y un número';
>   if (v.pass2 !== v.pass) e.pass2 = 'Las contraseñas no coinciden';
>   return e;
> }
> ```

> [!success]- Prueba de ejemplo
> ```jsx
> import { render, screen } from '@testing-library/react';
> import userEvent from '@testing-library/user-event';
> test('enviar vacío muestra errores', async () => {
>   render(<Registro />);
>   await userEvent.click(screen.getByRole('button', { name: /crear cuenta/i }));
>   expect(screen.getByText('Escribe tu nombre')).toBeInTheDocument();
>   expect(screen.getByLabelText(/nombre/i)).toHaveFocus();
> });
> ```
> En `vite.config` agrega `test: { environment: 'jsdom', setupFiles: './setup.js' }` y en `setup.js` importa `@testing-library/jest-dom/vitest`.

## Notas personales

_Qué me costó, errores que cometí, dudas._
