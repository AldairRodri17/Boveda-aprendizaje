---
proyecto: [GMM]
tipo: estructura
estado: vigente
tags: [estructura, reglas, frontend, gmm, guia-estilo]
origen: "Mi Estructura IA (archivo compartido por el usuario)"
---
# Guía GMM (original)

Índice: [[Estructura proyectos]] · Adaptación a SPA con módulos ES: [[Reglas Front - SPA]]

> [!note] Qué es esta nota
> Es la guía de estilo original de los módulos front-end de **GMM** (HTML + Service + Handler + Events, scripts globales en `window`), copiada tal cual. [[Ocelotl.Iris]] tomó de aquí el formato y la idea de capas, y los adaptó a módulos ES y [[lit-html]] (seis archivos y callbacks `Acciones` en lugar de `data-accion`). Para un proyecto **GMM** aplica esta guía completa; para una SPA nueva aplica [[Reglas Front - SPA]].

Eres un desarrollador de este proyecto. Todo módulo nuevo o modificado sigue **exactamente** esta guía. Si algo no está cubierto, imita los módulos ya migrados. No introduzcas frameworks, patrones ni librerías nuevas.

**Regla de oro: si tienes una duda, no inventes; pregunta.** Esto aplica a endpoints, nombres de campos del API, reglas de negocio, textos y comportamiento del legacy.

---

## 1. Stack y contexto global

- JavaScript plano ES2022 (clases, campos privados `#`, `static`, `async/await`, `??`, `?.`, desestructuración). **Sin** TypeScript y **sin** bundler: cada archivo se carga con `<script>` y expone su clase en `window`.
- Bootstrap solo para clases utilitarias (`btn btn-primary`, `right`…).
- Todo el texto visible está en **español**.

**Globales disponibles (no se reimplementan):**

| Global | Para qué |
| --- | --- |
| `Api` | HTTP (ver 6). `fnPostRespuesta(Url, Data, Campo?)` / `fnGetRespuesta(Url, Campo?)` para el sobre estándar; `fnPost` / `fnGet` / `fnPostArchivo` sin validar el sobre. |
| `ApiError` | Error de `Api`. **El usuario ya recibió el aviso**: no se vuelve a notificar. |
| `DetalleTramite` | Estado de la pantalla: `DetalleTramite.cargar(id)` (solo la pantalla contenedora), `await DetalleTramite.listo`, `DetalleTramite.isModoVista`, `DetalleTramite.recargar()`. |
| `_DetalleTramite` | Datos del trámite abierto, **solo lectura**: `idTramite`, `infoTramite` (CodEstatus, CodTipoTramite, CodTipoProducto, idTipoTramite, isLugarOcurrencia, isFechaOcurrencia, DatosSolicitud, FechaSolicitud…), `infoPersona` (idPersona…), `infoPoliza` (idPoliza, FechaBaja…), `infoTPA` (isBanorte, isGS), `InfoModulo` (isOperaciones, isMesaControl). |
| `Catalogos` | Constantes de dominio: `EstatusTramite`, `TipoTramite`, `SubtipoTramite`, `Estado`. **Nunca números mágicos**: `Catalogos.EstatusTramite.Cancelado`, no `3`. |
| `GeneralesHandler` | Solo estas funciones compartidas (estáticas): constantes `fnRegistrarConstantes(Constantes?)`; contacto `fnVerificarInfoContacto()`, `fnInfoContacto()`, `fnConfirmarContactos()`; firma `fnSolicitarFirma(Mensaje?)`. |

**Prohibido usar globales legacy** (`_Tramite`, `_infoTramite`, `isModoVista`, `getCodModulo()`, `codEstatusGlobal`, parámetros de la URL que ya existen en `_DetalleTramite`…). Todo sale de `_DetalleTramite` y `DetalleTramite`. Si algo no está ahí (p. ej. `idProductMarket`), se pregunta antes de leerlo de otro lado.

---

## 2. Estructura y responsabilidades

```
GMM/<Area>/<Modulo>/
├── <Modulo>.html
├── <Modulo>.css                (solo si tiene estilos)
├── JS/
│   ├── <Modulo>.Service.js
│   ├── <Modulo>.Handler.js
│   └── <Modulo>.Events.js
├── <Pestaña>/…                 (una carpeta por pestaña interna, ver 7.2)
└── Modals/<Modal>/
    ├── <Modal>.html
    └── JS/<Modal>.Service.js · <Modal>.Handler.js · <Modal>.Events.js   (igual que cualquier módulo)
```

- `<Area>`: `General` (lo compartido por varias pantallas), `MesaControl/Tramites/<Tramite>` u `Operaciones/Tramites/<Tramite>` (pantallas contenedoras y secciones propias de un trámite).

| Capa | Hace | No hace |
| --- | --- | --- |
| **HTML** | Declara la estructura y carga los 3 scripts | Lógica, estilos inline, scripts inline |
| **Service** | Endpoints y llamadas a `Api`; mapea nombres de parámetros y respuestas al dominio; convierte `ApiError` en `true/false/null` en guardados | DOM, notificaciones, leer `_DetalleTramite` |
| **Handler** | Lógica, render, validaciones, notificaciones, modales; lee `_DetalleTramite` | Registrar eventos |
| **Events** | Instancia el Handler, llama `init()` y conecta eventos → métodos del Handler | Lógica de cualquier tipo |
- Los scripts se cargan **siempre** en este orden: Service → Handler → Events.

### 2.1 Cada módulo es autocontenido

**Nada de un módulo vive fuera de sus archivos.** Sus endpoints, consultas, cálculos y validaciones están en su propio Service y Handler.

- Un módulo **nunca** llama al Service de otro módulo, ni a `GeneralesService`. Si necesita un endpoint que otro ya usa, lo declara en su propio `#EndPoint`.
- De otro módulo solo se usa su **contrato público**: `new XHandler().fnValidarX()` (7.7), un método público documentado para ese fin (p. ej. `new MovimientosHandler().fnAbrirRechazos()`, `new DatosSiniestroHandler().fnValores()`) y sus `static get Selectores`. Nunca se lee su DOM ni sus selectores internos para manipularlo.
- De Generales solo se usan las funciones de la tabla de la sección 1.
- Se quita todo lo que no se usa (métodos, endpoints, variables, archivos).

---

## 3. Nomenclatura

| Elemento | Regla | Ejemplo |
| --- | --- | --- |
| Clases | PascalCase + sufijo de capa; únicas en `window` | `DocumentosHandler`, `CartasSiniestralidadOperacionesHandler` |
| Archivos | `<Modulo>.<Capa>.js` | `Documentos.Handler.js` |
| IDs HTML | PascalCase; botones de acción `Modulo__Accion` | `FormCuentaBancaria`, `Facturas__CargarXML` |
| Clases CSS | kebab-case con prefijo del módulo | `facturas-lista` |
| Métodos | **Obligatorio** `fn` + PascalCase (públicos, privados, `async`, estáticos). Excepciones: `init()`, `constructor` y los `static get` | `fnLigarArchivo`, `#fnCargarCuentas` |
| Variables y parámetros | **PascalCase**, incluso en callbacks. Excepciones: `_` (sin usar), `e` (evento), `error` (catch) y los campos del API al desestructurar (`{ id, name }`, `{ action, row }`) | `Faltantes`, `(_, Campo) => …` |
| Booleanos | Prefijo `is` (también campos privados) | `isCompleta`, `#isGuardando` |
| Variables con HTML | Prefijo `html` | `htmlTarjeta` |
| Instancia en Events | `_<Modulo>Handler` | `_FacturasHandler` |
| Opciones para combos | `Opciones` + entidad | `OpcionesEstados` |
| Alias | `S` (selectores), `M` (mensajes), `P` (parámetros), `L` (límites) | `const S = XHandler.#Selectores` |
| Campos e IDs del API | Tal como vienen del API | `idTramite`, `codMetodoPago` |
| Métodos del Service | Lo que regresan; plural para listas | `fnCuentas`, `fnExisteCLABE`, `fnGuardarCuenta` |
| Acciones por tipo de trámite | `<Verbo><TipoTramite>` | `#fnEnviarProgramacionServicios` |
| Métodos públicos para otros módulos | `fnValidar<Seccion>`, `fnValores`, `fnAbrir<X>` | `fnValidarInformacionBancaria` |

---

## 4. Formato

**General**

- Indentación de 4 espacios. **Sin punto y coma.** Comillas simples en JS.
- En templates HTML dentro de JS, comillas simples en los atributos.
- Archivos con fin de línea CRLF; `.html` y `.css` con BOM UTF-8. El archivo termina con salto de línea.
- La clase no va indentada: `window.X = class X {` empieza en la columna 0.
- Los IDs se comparan con `==` (porque `dataset` siempre es string) o con `String(...)`.

**Líneas en blanco (nunca dos seguidas)**

- 1 después de abrir la clase; 1 entre bloques `static #` y entre métodos.
- 0 al inicio y al final del cuerpo de un método y antes de cerrar la clase.
- Dentro de un método, 1 solo para separar pasos: preparar → ejecutar → resolver.
- Separadores de sección: `// ───────────────────────── Sección ─────────────────────────`.

**Templates HTML en JS**

- Se abren en la línea de la variable (`const htmlX = ``), un atributo por línea y el cierre en la última. Se regresa la variable.
- Los datos del API se escapan siempre antes de insertarlos.

**Llamadas y objetos**

- Más de 2 propiedades o más de ~120 caracteres: una propiedad por línea.
- `if` de una instrucción en una línea sin llaves: `if (!Registro) return`. Con 2 o más, con llaves.

**Comentarios**

- Solo para un contrato o un porqué, en español y en JSDoc de una línea: `/** true si se guardó; false si el API falló (el aviso ya salió). */`.
- Si reemplaza una función legacy: `(antes: NombreLegacy)`.

---

## 5. HTML

```html
<link href="GMM/<Area>/<Modulo>/<Modulo>.css" rel="stylesheet" />

<div id="Form<Modulo>" class="<modulo>-fila">
    <!-- campos del módulo -->
</div>

<div id="Lista<Modulo>" class="<modulo>-lista"></div>

<script type="text/javascript" src="GMM/<Area>/<Modulo>/JS/<Modulo>.Service.js"></script>
<script type="text/javascript" src="GMM/<Area>/<Modulo>/JS/<Modulo>.Handler.js"></script>
<script type="text/javascript" src="GMM/<Area>/<Modulo>/JS/<Modulo>.Events.js"></script>
```

Reglas:

- El `<link>` solo si el `.css` existe (nada de links comentados).
- Los campos llevan `id`; el `name` es el nombre que espera el API cuando el formulario se serializa.
- Nunca alerts, estilos inline ni scripts inline.

---

## 6. Service

```jsx
window.<Modulo>Service = class <Modulo>Service {

    static #EndPoint = Object.freeze({
        Cuentas: 'API/api/Tramite/MesaControl_TramitePersonaGetCuentas',
        GuardarCuenta: 'API/api/Tramite/MesaControl_TramiteAddCuentaBancaria'
    })

    static #Parametros = Object.freeze({
        CuentaVacia: '00000000-0000-0000-0000-000000000000'
    })

    /** { Cuentas, Documentos } de la persona. */
    async fnCuentas({ idPersona }) {
        const Tablas = await Api.fnPostRespuesta(<Modulo>Service.#EndPoint.Cuentas, {
            idCuenta: <Modulo>Service.#Parametros.CuentaVacia,
            idPersona
        })
        return {
            Cuentas: Tablas.Cuentas ?? [],
            Documentos: Tablas.Cuentas1 ?? []
        }
    }

    /** idCuenta nueva; null si el API falló (el aviso ya salió). */
    async fnGuardarCuenta({ idPersona, CLABE, Titular }) {
        try {
            const Cuentas = await Api.fnPostRespuesta(<Modulo>Service.#EndPoint.GuardarCuenta,
                { idPersona, ClabeBancaria: CLABE, NombreTitular: Titular }, 'Cuenta')
            return Cuentas[0]?.idCuenta ?? null
        } catch (error) {
            if (!(error instanceof ApiError)) throw error
            return null
        }
    }
}
```

Reglas:

- `static #EndPoint` congelado; la llave se llama como el método sin `fn`. Códigos del API (codTipoFile, GUID vacío, textos que regresa el API) en `static #Parametros`.
- Cada método recibe **un objeto desestructurado** y arma el payload solo con esos campos. **El mapeo de nombres va aquí** (`{ ClabeBancaria: CLABE }`, `{ IdTramite: idTramite }`).
- **Normaliza la respuesta aquí:** el Handler recibe datos de dominio (`[]`, `{}`, un `id`, un boolean), nunca `dsRespuesta`.
- Qué método de `Api` usar:
    - `fnPostRespuesta` / `fnGetRespuesta`: sobre estándar. Con `Campo` regresa esa tabla o `[]`; sin `Campo`, todas las tablas (`Datos`, `Datos1`…), que se leen con `?? []`.
    - `fnPost` / `fnGet`: el API no usa el sobre, o el dato importa en `Mensaje`/`IsOk` (p. ej. el estatus del SAT al leer un XML). El Service interpreta la respuesta y regresa algo de dominio.
    - `fnPostArchivo(Url, FormData)`: multipart a un endpoint de archivos.
- Consultas simples: regresan directo la Promise de `Api` (sin `async`). `async` solo si transforma o usa `try/catch`.
- `try/catch` solo en **guardados** (y en consultas donde el legacy trataba la falla como "sin resultado", con comentario): convierte `ApiError` en `false`/`null`; cualquier otro error se relanza. El Service nunca notifica.
- **No mandes vacíos a parámetros opcionales con validación de tipo** (GUID, fecha): si no hay valor, no se envía la llave. Revisa en el back qué valida `EvalInput`.
- Agregar y Eliminar sobre un mismo endpoint: públicos delgados que delegan en un privado con `isActive: 1 | 0`.

---

## 7. Handler

```jsx
window.<Modulo>Handler = class <Modulo>Handler {

    #Service = new <Modulo>Service()
    #Registros = []
    #isGuardando = false

    static #Limites = Object.freeze({
        ArchivosPorDocumento: 5
    })

    static #Selectores = Object.freeze({
        Tab: '#Tab<Modulo>',
        Form: '#Form<Modulo>',
        Accion: '#<Modulo>__Accion',
        Lista: '#Lista<Modulo>',
        AccionRegistro: '[data-accion]'
    })

    static #Mensajes = Object.freeze({
        Faltantes: 'Llena los datos solicitados',
        Guardado: 'Información guardada correctamente',
        EliminarMensaje: '¿Seguro que desea eliminar el registro?',
        NoGuardado: Nombre => `No se pudo guardar "${Nombre}". Vuelve a intentarlo.`
    })

    static get Selectores() {
        return <Modulo>Handler.#Selectores
    }

    async init() {
        await DetalleTramite.listo

        await this.#fnCargarRegistros()
        this.#fnModoVista()
    }

    #fnEditable() {
        return !DetalleTramite.isModoVista
    }

    // ───────────────────────── Listado ─────────────────────────

    async #fnCargarRegistros() {
        this.#Registros = await this.#Service.fnRegistros({ idTramite: _DetalleTramite.idTramite })
    }

    // ───────────────────────── Acciones (Events) ─────────────────────────

    /** click en un botón de un registro. */
    async fnAccionRegistro({ accion, idRegistro }) {
        const Registro = this.#Registros.find(Item => String(Item.idRegistro) == idRegistro)
        if (!Registro) return

        if (accion == 'Eliminar') return this.#fnEliminar(Registro)
    }

    async #fnEliminar(Registro) {
        const isEliminado = await this.#Service.fnEliminar({ idRegistro: Registro.idRegistro })
        if (!isEliminado) return

        await this.#fnCargarRegistros()
    }

    // ───────────────────────── Para otros módulos ─────────────────────────

    /** Funciona sin init: true si la sección está completa (o no aplica). */
    async fnValidar<Modulo>() {
        const Registros = await this.#Service.fnRegistros({ idTramite: _DetalleTramite.idTramite })
        const isCompleta = Registros.length > 0
        return isCompleta
    }
}
```

### 7.1 Estructura de la clase

1. Campos de instancia: `#Service` y otros privados (`#isGuardando`).
2. Bloques `static #` con `Object.freeze`, solo los que se usen, en este orden: `#Limites` → constantes de dominio propias (`#Parametros`, `#Pasos`, `#Origen`, `#MetodoPago`…) → `#Rutas` → `#Selectores` → `#Mensajes` → `#Modals`.
3. `static get Selectores()` (lo usa Events).
4. `async init()` (sin `constructor`).
5. Métodos agrupados por separador: carga, render, acciones (los públicos que llama Events), guardado y "Para otros módulos".

### 7.2 Pantalla contenedora (encabezado + pestañas de un trámite)

- **HTML:** `<div id="Header<Pantalla>">` + una pestaña por sección. Al final: `Detalletramite.js`, `Generales.Service.js`, `Generales.Handler.js` y su Handler y Events.
- **Handler:** `#Selectores`, `#Rutas` y, si aplica, `#Mensajes` y listas de tipos. `init()` en este orden:
    1. `GeneralesHandler.fnRegistrarConstantes()`
    2. `await DetalleTramite.cargar(idTramite)` con el `idTramite` de la URL
    3. Carga del encabezado.
- Lo que falta migrar se anota con un `console.log(#Mensajes.MigracionPendiente)` en `init()`, sin cargar scripts ni globales legacy.
- IDs de pestañas fijos (`TabsTramite`, `TabDatosSolicitud`, `TabDocumentos`, `TabMovimientos`…): Headers y otras secciones los usan.
- **Pestañas internas** (p. ej. `General/Movimientos`): una carpeta por pestaña, cada una módulo completo; la raíz solo arma las pestañas y un Handler sin Service.

### 7.3 Sección dentro de otra sección

(Ejemplo: `DatosSiniestro` dentro de `DatosProgramacion`.)

- La sección contenedora la carga en su contenedor; la sección hija se inicializa sola (su Events) y carga lo suyo.
- La hija expone métodos públicos que funcionan sin `init`: `fnValores()` (lo capturado) y `fnValidar<Hija>()` (valida y marca sus campos). La contenedora los usa con `new <Hija>Handler()` y es quien guarda.

### 7.4 Reglas generales

- **Ningún texto, selector, ruta, límite o número mágico literal dentro de un método.** Mensajes dinámicos: funciones flecha en `#Mensajes`.
- Los bloques estáticos se usan con el nombre de la clase (nunca `this.constructor`); alias local si se usan varias veces.
- `init()`: `await DetalleTramite.listo` → constantes → cargas. Las cargas independientes van en `Promise.all`.
- Retorno temprano en validaciones y flujos.
- Notificaciones y modales solo en el Handler.
- **Sin `try/catch` ni `try/finally`** para el API: el Handler recibe `true/false/null` o datos. Para quitar un estado de carga se usa `.finally()` sobre la promesa: `await this.#Service.fnBuscar(…).finally(() => this.#isBuscando = false)`.
- Evitar dobles envíos con un booleano `#isGuardando`.
- Un método `async` evaluado en un `if` siempre lleva `await`.
- Restricciones por estatus, producto o TPA en un método `#fnEditable()`; modo vista en `#fnModoVista()` (campos `disabled`/`readonly`).

### 7.5 Formularios

- Catálogos: el Service regresa la lista y el Handler la mapea a las opciones del combo.
- Combos en cascada: limpiar el hijo, retorno temprano si el padre está vacío, consultar y llenar.
- Atributos que dependen de datos (p. ej. la fecha máxima con `_DetalleTramite.infoPoliza.FechaBaja`) se asignan en un `#fnAsignar<X>()`.

### 7.6 Listas

- **Listas de HTML** (tarjetas, botones, renglones): limpiar el contenedor y agregar cada elemento con `forEach`. **Nunca `map(...).join('')`.**
- Acciones de cada elemento como `<button data-accion="…" data-id-…>`; Events delega el click en el contenedor y el Handler resuelve en `fnAccion<Entidad>({ accion, id… })`.
- Varios renglones de texto en un mensaje: `Lista.join('\n')` + CSS `white-space: pre-line` (no HTML).

### 7.7 Validación de sección (contrato para otros módulos)

**Toda sección expone `fnValidar<Seccion>()`:**

- `async`, regresa `true` si está completa y `false` si falta algo.
- **Funciona sin `init()`**: consulta su Service (lo guardado) y lee su DOM si hace falta.
- **Si no aplica al trámite, regresa `true`.**
- **Marca su pestaña** y, si puede, sus campos. Al guardar bien, quita la marca.
- No notifica ni abre modales.

Quien la usa (Headers) la llama y junta varias en una validación:

```jsx
async #fnValidarDocumentos() {
    const _DocumentosHandler = new DocumentosHandler()
    return _DocumentosHandler.fnValidarDocumentos()
}
```

### 7.8 Acciones por tipo de trámite (Headers)

- `#Botones` se configura por **Estatus + TipoTramite**; cada tipo con pasos propios tiene su `Accion` y su método `#fn<Verbo><TipoTramite>` (p. ej. `EnviarProgramacionServicios`, `AceptarCartaSiniestralidad`).
- Nunca `if (TipoTramite == …)` dentro de un método. Lo que comparten va en "Pasos comunes" (`#fnEnviarADictamen`, `#fnContactoYFirmaConfigurada`, `#fnAvisarAutorizacion`, `#fnRegresarAlModulo`).
- Flujo típico de una acción: validación de secciones → contacto y firma configurada (`fnVerificarInfoContacto`) → `GeneralesHandler.fnSolicitarFirma()` → llamada al Service con estado de carga → avisar → regresar al módulo.

### 7.9 Modales

- **Un modal es un módulo igual que una sección:** su `.html` con los tres scripts y un Handler sin `constructor`.
- Las rutas van en `#Modals`; los datos los manda quien lo abre, nunca en globales.
- Los IDs son únicos en la página: el mismo módulo no se abre dos veces al mismo tiempo.
- Nunca `confirm()` ni `alert()`. Contactos: `GeneralesHandler.fnConfirmarContactos()`.
- Un modal legacy no migrado se puede abrir con su función legacy (`fncOpenOverlayHTML`) solo mientras se migra.

---

## 8. Events

```jsx
const _<Modulo>Handler = new <Modulo>Handler()
_<Modulo>Handler.init()
```

Reglas:

- Se ejecuta cuando el documento está listo, también en los modales. `init()` sin `await`.
- Elementos generados por un render: **delegación** desde su contenedor.
- Cada listener llama a **un** método del Handler (puede pasarle un objeto armado con el `dataset`).
- `function (e)` solo si se necesita `this`; si no, flecha (`e.currentTarget` para el elemento).

---

## 9. Módulos compartidos (Generales)

- Clases sin estado usadas por varias pantallas: métodos **estáticos** (`GeneralesHandler.fnX()`).
- Valores de otro archivo: `static get` para no depender del orden de carga.
- `Api`, `DetalleTramite`, `Catalogos` son infraestructura: se usan, no se copian.

---

## 10. Cómo migrar un módulo legacy

1. **Bajar toda la lógica** del legacy (HTML y JS, incluidos sus modales y lo que toma del contenedor): carga, eventos, validaciones, endpoints con sus parámetros y tablas de respuesta, mensajes y estados.
2. **Revisar el back** de cada endpoint (controlador → clase → SP): qué valida (`EvalInput`, tipos, obligatorios), qué tablas regresa, cuándo regresa `IsOk = false`, permisos.
3. **Listar código muerto y errores** del legacy y preguntar antes de omitirlos o corregirlos.
4. **Preguntar** todo lo que no se pueda deducir. Nada se inventa.
5. Programar con esta guía, conservando textos y reglas del legacy salvo indicación.
6. Probar (sintaxis, flujo con respuestas simuladas) y reportar qué se probó y qué falta verificar contra el API real.

Reglas de alcance:

- Lo que no está en `GMM/` se queda como está; si se necesita en GMM, se hace una copia en GMM y se migra la copia.
- Funciones legacy de otros módulos se llaman con su nombre original (no se renombran).
- Al terminar, se borra el legacy que ya no se usa.

---

## 11. Prohibido

- Frameworks, `fetch` u otras llamadas HTTP directas (siempre `Api`).
- `alert`, `confirm`, `onclick` inline, scripts inline.
- Literales de textos, selectores, endpoints, rutas o **números mágicos**.
- Lógica en Events, eventos en el Handler, DOM o notificaciones en el Service.
- `try/catch` o `try/finally` del API en el Handler; tragar errores que no sean `ApiError`; volver a notificar un `ApiError`.
- Globales legacy o modificar `_DetalleTramite`.
- Llamar a `GeneralesService` o al Service de otro módulo; manipular el DOM de otro módulo.
- `map(...).join('')` para armar HTML.
- Variables en camelCase o minúscula (`data`, `docs`, `x`), métodos sin `fn`, booleanos sin `is`.
- `this.constructor` para los `static #`; `if` con `async` sin `await`.
- Métodos vacíos, código muerto, links o código comentado, dobles líneas en blanco, clase indentada.
- Mandar `''` a parámetros opcionales que el back valida como GUID o fecha.
- Inventar: endpoints, campos, reglas o textos que no estén en el legacy, en el back o confirmados.

---

## 12. Checklist antes de entregar

- [ ]  Los 3 JS existen, se cargan Service → Handler → Events y pasan `node --check`.
- [ ]  `window.<Clase>` única; métodos con `fn` (salvo `init`, `constructor`, getters); variables PascalCase; booleanos `is`.
- [ ]  Sin literales: todo en bloques `static #` o `Catalogos`.
- [ ]  Eventos delegados si son de un render, una llamada al Handler.
- [ ]  Service: payload mapeado, respuesta normalizada, guardados con `true/false/null`; sin vacíos en parámetros GUID/fecha opcionales.
- [ ]  Handler: sin `try`; restricciones en `#fnEditable()`; modo vista aplicado; doble envío evitado.
- [ ]  Listas con `forEach` (nunca `map(...).join('')`); datos del API escapados.
- [ ]  Los modales son módulos normales, sin `constructor`.
- [ ]  La sección expone `fnValidar<Seccion>()` (sin `init`, `true` si no aplica, marca su pestaña).
- [ ]  Nada fuera de sus archivos; legacy sin uso borrado.
- [ ]  Formato: 4 espacios, sin `;`, sin dobles líneas en blanco, CRLF, BOM en html/css.
- [ ]  Reporte: qué se probó, qué falta verificar contra el API real y qué dudas quedan.
