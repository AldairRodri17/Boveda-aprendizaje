---
proyecto: [Ocelotl.Iris]
tipo: estructura
estado: vigente
tags: [estructura, reglas, frontend, javascript, gmm, lit-html]
actualizado: 2026-11-08
contenido: completo
---
# Reglas Front - SPA

Índice: [[Estructura proyectos]] · Referencia viva: [[Ocelotl.Iris]] · Aprendizajes: [[Ocelotl.Iris - Aprendizajes]] · Origen del formato: [[Guía GMM (original)]]

> [!important] Resumen para Claude
> 1. JavaScript plano con **módulos ES**, sin framework; render con [[lit-html]]; desarrollo y build con **[[Vite]]** (`npm run dev` / `npm run build`).
> 2. Cada pantalla es una carpeta `features/<x>/` con **seis archivos**: html · css · service · view · handlers · events.
> 3. La vista dispara solo lo que le pasan en **`Acciones`** (callbacks). Nada de `data-accion` / `data-valor`.
> 4. Formato **GMM**: `fn` + PascalCase, variables PascalCase, booleanos `is`, sin `;`, 4 espacios, comillas simples, constantes con `Object.freeze`.
> 5. Si hay duda: **pregunta; no inventes.**

## 1. Estructura
> [!info] Variante con Vite
> Esta es la variante **con bundler** (Vite + módulos ES). La variante **sin bundler** (tres archivos `Service` · `Handler` · `Events` cargados con `<script>`) está en [[Reglas para crear proyectos]] §4. `ARQUITECTURA.md` dice cuál usa cada proyecto.


```text
vite.config.mjs        root src/ · base './' (file://) · salida dist/ · servidor localhost:5173 (strictPort)
scripts/dev.mjs        Vite (API) + Electron con IRIS_DEV_URL
electron/main.js       loadURL(IRIS_DEV_URL) en desarrollo · loadFile(dist/index.html) empaquetada
dist/                  build (no se versiona)
src/
├─ index.html          CSP sin scripts inline (script-src 'self') · <link> a styles/main.css · <script type=module src=./app.js>
├─ app.js              Arranque: sesión, router
├─ core/               router.js · rutas.js (Path, Feature, Titulo, Icono, Roles) · hermes.client.js · robot.events.js · app.service.js
├─ shared/components/  fnUiButton, fnUiInput, fnUiSelect, fnUiModal, fnUiConfirmacion, fnUiTabs, fnUiPaginacion, fnUiPendiente
├─ features/<x>/       x.html · x.css · x.service.js · x.view.js · x.handlers.js · x.events.js
└─ styles/
```
Ver [[SPA y router por hash]] y [[Vite]].

**Vite, reglas:**
- Librerías de `node_modules` con `import` normal (`'lit-html'`, `'@microsoft/signalr'`); nada de copiarlas a `vendor/` ni de usar `window.X`.
- Nada de `import()` con rutas armadas en tiempo de ejecución: el router usa `import.meta.glob('../features/*/*.events.js')`, más `*.html` con `?raw` y `*.css`.
- Archivos usados solo por ruta en texto (íconos en `.html`): en `src/assets`, copiados al build por el plugin de `vite.config.mjs`.
- El puerto de desarrollo es fijo (`strictPort`) y debe estar en los orígenes permitidos de CORS del backend.
- `base: './'` siempre (Electron abre `dist/` con `file://`).

## 2. El molde (una pantalla)
| Archivo | Hace | No hace |
|---|---|---|
| `x.html` | Estructura estática y contenedores con `id` en PascalCase | Lógica, `onclick`, scripts o estilos inline |
| `x.css` | Estilos colgando de `.pagina-x` | — |
| `x.service.js` | Sus endpoints (`EndPoint` congelado); recibe **un objeto** (`{ RobotId, Signal }`); mapea nombres del API; normaliza (`?? []`) | DOM, toasts, importar el service de otra pantalla |
| `x.view.js` | Funciones puras `(Estado, Acciones) => html\`…\`` y `fnPintar(Root, Estado, Acciones)` | Llamar al API, cambiar el estado, importar el handler |
| `x.handlers.js` | `Estado`, `fnActualizar()` y las acciones: validar, llamar al service, avisar, actualizar | Registrar eventos, armar HTML |
| `x.events.js` | Arma `Acciones`; `mount(Root)` / `unmount()`; suscripciones a eventos en vivo | Lógica de cualquier tipo |

**Ciclo:** el router carga la pantalla → `mount(Root)` crea un [[AbortController]] → el handler pinta → `@click=${Acciones.fnX}` → el handler cambia `Estado` con `fnActualizar` y vuelve a pintar → `unmount()` hace `abort()` (cancela peticiones y suscripciones).

```js
// x.events.js
const Acciones = Object.freeze({
    fnRecargar: XHandler.fnCargar,
    fnConectar: XHandler.fnConectar
})
```
Detalle completo: [[Molde Service-View-Handlers-Events]].

## 3. Reglas
- **Autocontenido:** una pantalla nunca importa el service de otra; si necesita un endpoint, lo declara en su `EndPoint`. Lo compartido vive en `core/`.
- **Solo los `*.service.js`** llaman a `Hermes.fnGet/fnPost`. No mandar llaves vacías a parámetros opcionales.
- **Sin literales** dentro de funciones: `Mensajes` (los dinámicos como flechas), `Selectores`, `Limites`, `Reglas`, `EndPoint`, todos con `Object.freeze`.
- **Retorno temprano;** `isEnviando` para evitar el doble envío; guardar el `Signal` de la acción y salir si llegó `fnEsCancelado(error)`.
- **Componentes controlados** `fnUi*`: reciben valores y callbacks; no guardan estado propio ([[Componentes controlados]]). Modales con `<dialog>` ([[Elemento dialog]]).
- **Seguridad (Electron):** sandbox, `contextIsolation`, preload mínimo con IPC tipo pull, CSP sin scripts inline, enlaces externos solo por `fnAbrirExterno` (https) ([[Electron]] · [[Preload y contextBridge]] · [[Content Security Policy]]).
- **Roles** en `rutas.js` (`Roles`), con guardias en el router.
- **Prohibido:** `alert`, `confirm`, `innerHTML` con datos del API, `window.App`, frameworks, `data-accion`, `map(...).join('')` para armar HTML, datos inventados.

## 4. Formato (de la guía GMM)
| Elemento | Regla | Ejemplo |
|---|---|---|
| Funciones | `fn` + PascalCase (salvo `mount`, `unmount`) | `fnCargarRobots` |
| Variables, constantes, parámetros | PascalCase, también en callbacks; excepciones `_`, `e`, `error` y campos del API al desestructurar | `(Robot) => Robot.robotId` |
| Booleanos | prefijo `is` | `isEnviando` |
| Plantillas | prefijo `html` | `htmlTarjeta` |
| IDs HTML | PascalCase | `FormDispensar` |
| Clases CSS | kebab-case con prefijo de la pantalla | `ingresos-resultado` |

- 4 espacios, **sin punto y coma**, comillas simples (también en atributos de plantillas), `==` para comparar ids.
- `if` de una instrucción en una línea sin llaves.
- Separadores `// ───────────────────────── Sección ─────────────────────────`; nunca dos líneas en blanco seguidas.
- Comentarios solo para un contrato o un porqué, en JSDoc de una línea.

## 5. Agregar una pantalla
1. Copiar `features/configuracion` a `features/<nueva>` y renombrar.
2. Agregarla a `core/rutas.js` (`Path`, `Feature`, `Titulo`, `Icono`, `Roles`).
3. Declarar sus endpoints en su service; probar con `npm run dev` (recarga en caliente) y con respuestas simuladas. El router la encuentra solo (`import.meta.glob`): no hay que registrar archivos.

## 6. Checklist antes de entregar
- [ ] Seis archivos (o los que apliquen) y ninguno hace lo que "no hace".
- [ ] La vista solo usa `Acciones`; sin `data-accion`.
- [ ] Sin literales sueltos; formato GMM.
- [ ] `unmount` cancela todo; sin fugas de suscripciones.
- [ ] `npm run build` pasa sin errores y `npm start` abre la app desde `dist/`. Reportar qué se probó y qué falta probar contra el API real.
