---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: JavaScript y Web
herramienta: Node.js o consola del navegador
tags: [javascript-web, laboratorio]
---
# Lab 2 - Predecir la salida de JavaScript (coerción, closures y this)

Parte de: [[04-JavaScript-Web/02 Básico de JavaScript/_Índice Básico de JavaScript|Básico de JavaScript]]

**Cubre:** [[Coerción de tipos]] · [[== vs ===]] · [[Valores truthy y falsy]] · [[Hoisting]] · [[Closures]] · [[this en JavaScript]] · [[call, apply y bind]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Entrenar el ojo: escribir en papel lo que imprime cada fragmento **antes** de ejecutarlo, y luego comprobarlo. El objetivo es que al menos 9 de 10 predicciones salgan bien.

## Pasos

1. Crea `predicciones.md` en esta carpeta con una tabla: fragmento, tu predicción, resultado real, por qué.
2. Fragmento 1: `console.log([] == false, [] ? 'sí' : 'no');`
3. Fragmento 2: `console.log('b' + 'a' + +'a' + 'a');`
4. Fragmento 3: `console.log(typeof x, typeof y); var x = 1; let y = 2;` (guárdalo en un archivo aparte; ¿hay error?).
5. Fragmento 4: `for (var i = 0; i < 3; i++) setTimeout(() => console.log(i));`
6. Fragmento 5: `const o = { n: 1, f() { return () => this.n; } }; console.log(o.f()());`
7. Fragmento 6: `const o = { n: 1, f() { return function () { return this?.n; }; } }; console.log(o.f()());` (en un módulo `.mjs`).
8. Fragmento 7: `function s() { return this.v; } console.log(s.call({ v: 2 }), s.bind({ v: 3 }).call({ v: 4 }));`
9. Fragmento 8: `console.log(null ?? 0 || 'x');` (¿es válido?) y luego `console.log((null ?? 0) || 'x');`
10. Fragmento 9: `let a = 1; { let a = 2; } console.log(a);`
11. Fragmento 10: `const f = (() => { let c = 0; return () => c++; })(); f(); f(); console.log(f());`
12. Ejecuta cada fragmento con `node archivo.mjs` (o en la consola) y llena la columna de resultados.

## Criterios de terminado

- [ ] `predicciones.md` tiene los 10 fragmentos con predicción, resultado y explicación propia.
- [ ] Al menos 9 de 10 predicciones coinciden con el resultado.
- [ ] Para cada fallo escribiste qué regla no recordabas y enlazaste la nota correspondiente.

## Solución

> [!success]- Resultados 1 a 5
> 1. `true sí`: con `==`, `[]` pasa a `""` y luego a `0`, igual que `false`; pero como valor booleano `[]` es truthy.
> 2. `baNaNa`: `+'a'` es `NaN` y se concatena.
> 3. `undefined` para `x` y luego `ReferenceError` para `y` (TDZ). `typeof` no protege de la TDZ.
> 4. `3 3 3`: `var` es una sola variable compartida.
> 5. `1`: la flecha toma `this` de `f`, que se llamó como `o.f()`.

> [!success]- Resultados 6 a 10
> 6. `undefined`: la función normal se llama sin objeto; en módulo `this` es `undefined`.
> 7. `2 3`: `call` cambia `this`, pero una función ligada ignora el `call` posterior.
> 8. La primera línea es **SyntaxError**: no se puede mezclar `??` con `||` sin paréntesis. La segunda imprime `x`.
> 9. `1`: el `a` interno vive solo en su bloque.
> 10. `2`: `c++` devuelve el valor antes de incrementar (0, 1, 2).

## Notas personales

_Qué me costó, errores que cometí, dudas._
