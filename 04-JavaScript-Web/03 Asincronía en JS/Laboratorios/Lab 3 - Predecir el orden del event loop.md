---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: JavaScript y Web
herramienta: Node.js o consola del navegador
tags: [javascript-web, laboratorio]
contenido: completo
---
# Lab 3 - Predecir el orden del event loop

Parte de: [[04-JavaScript-Web/03 Asincronía en JS/_Índice Asincronía en JS|Asincronía en JS]]

**Cubre:** [[Event loop]] · [[Microtareas y macrotareas]] · [[Timers]] · [[Promesas]] · [[async y await]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Predecir el orden de los logs en fragmentos que mezclan código síncrono, promesas, `await` y timers, y explicar cada orden con el modelo pila → microtareas → tarea.

## Pasos

1. Crea `orden.mjs` en esta carpeta.
2. Fragmento A:
   ```js
   console.log(1);
   setTimeout(() => console.log(2));
   Promise.resolve().then(() => console.log(3));
   console.log(4);
   ```
3. Fragmento B:
   ```js
   async function a() { console.log('a1'); await b(); console.log('a2'); }
   async function b() { console.log('b'); }
   console.log('inicio'); a(); console.log('fin');
   ```
4. Fragmento C:
   ```js
   setTimeout(() => console.log('T1'));
   setTimeout(() => { console.log('T2'); Promise.resolve().then(() => console.log('P-dentro-T2')); });
   setTimeout(() => console.log('T3'));
   ```
5. Fragmento D:
   ```js
   Promise.resolve().then(() => console.log('p1')).then(() => console.log('p2'));
   Promise.resolve().then(() => console.log('q1')).then(() => console.log('q2'));
   ```
6. Fragmento E:
   ```js
   const p = new Promise(r => { console.log('ejecutor'); setTimeout(() => r('listo')); });
   p.then(console.log);
   queueMicrotask(() => console.log('micro'));
   console.log('sync');
   ```
7. Escribe tu predicción de cada uno en un comentario, luego ejecútalos (uno a la vez) con `node orden.mjs`.
8. En la pestaña Performance de Chrome graba la ejecución del fragmento C en una página de prueba y ubica las tres tareas.

## Criterios de terminado

- [ ] Escribiste la predicción de los 5 fragmentos antes de ejecutarlos.
- [ ] Al menos 4 de 5 coinciden con lo real.
- [ ] Para cada fragmento hay una explicación de una o dos líneas que menciona pila, microtareas o tareas.
- [ ] Identificaste en Performance las tareas de los `setTimeout` del fragmento C.

## Solución

> [!success]- Resultados
> - A: `1 4 3 2`.
> - B: `inicio a1 b fin a2`: `b()` corre síncrona; lo que sigue al `await` es microtarea.
> - C: `T1 T2 P-dentro-T2 T3`: tras cada tarea se vacían las microtareas antes de la siguiente tarea.
> - D: `p1 q1 p2 q2`: cada `.then` encadenado se encola cuando el anterior termina, así se intercalan.
> - E: `ejecutor sync micro listo`: el ejecutor es síncrono; la promesa se resuelve en una tarea posterior.

## Notas personales

_Qué me costó, errores que cometí, dudas._
