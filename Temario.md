---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
tags: [temario, conocimiento, ruta-senior]
---

# Temario de la biblioteca de conocimiento

> Cada línea con `[[ ]]` es una nota independiente. Los enlaces a notas que aún no existen aparecen como pendientes, así que este archivo es índice y lista de avance.
> Cada área va de lo más básico a lo más completo. Marca `[x]` cuando la nota esté escrita y revisada.
> Objetivo: llegar a **senior**. Los hobbies y temas de interés se agregan después en su propia sección.

## Leyenda

| Marca | Significado |
|---|---|
| ⭐ | Imprescindible para senior (oleada 1) |
| 🛠 | Ya lo usas en un proyecto: enlazar la nota con el proyecto (oleada 1) |
| _(existe)_ | La nota ya está en `Conocimiento/`: se complementa, no se duplica |
| sin marca | Completar después (oleadas 2 y 3) |

**Orden sugerido:** ⭐ y 🛠 → lo que piden tus reglas → el resto.

## Plantilla de frontmatter (todas las notas de conocimiento)

```yaml
---
proyecto: Conocimiento
tipo: concepto | herramienta | patron | servicio | guia | certificacion
nivel: basico | intermedio | avanzado
estado: pendiente | borrador | revisada
proyectos: []   # [[Itzamna.Dionisio]], [[Pochteca.Nomisma]]...
tags: []
---
```

## Estructura del vault

```
00-Inbox/
01-Reglas/                    ← hoy: Estructura proyectos/
02-Fundamentos/
03-CSharp-DotNet/
04-JavaScript-Web/
05-React/
06-SQL-Server/
07-Azure/
08-Automatizacion-DevOps/
09-Arquitectura-Patrones/
10-Radar-Herramientas/
11-Proyectos/                 ← hoy: una carpeta por proyecto en la raíz
12-Seguridad/
13-Testing-Calidad/
14-Carrera-Liderazgo/
15-Java/
16-Laboratorios-Practica/
17-Power-BI/
18-Certificaciones/
19-Redes/                     ← nuevo
20-Angular/                   ← nuevo
21-IA-Machine-Learning/       ← nuevo
22-Integraciones/             ← nuevo (pagos, correo, documentos, escritorio)
30-Fotografia/                ← hobby
31-Video-Audio/               ← hobby
32-Edicion-Foto/              ← hobby
33-Edicion-Video/             ← hobby
34-Equipo/                    ← hobby
35-Drones/                    ← hobby
36-Impresion-3D/              ← hobby
37-Modelado-3D-Tecnico/       ← hobby
38-Modelado-3D-Artistico/     ← hobby
39-IA-Modelado-3D/            ← hobby
Plantillas/
```

> **Migración:** hoy las notas están en `Conocimiento/` (plano). Mover un archivo no rompe enlaces si el nombre no cambia. Antes de mover se revisa la tabla "qué va a dónde".

---

## 01 · Reglas

- [x] [[Reglas para crear proyectos]] _(existe)_ ⭐
- [x] [[Reglas Back - .NET]] _(existe)_ ⭐
- [x] [[Reglas Front - SPA]] _(existe)_ ⭐
- [x] [[Guía GMM (original)]] _(existe)_
- [x] [[Estructura proyectos]] _(existe, índice de reglas y decisiones)_
- [ ] [[Reglas de comentarios]]
- [ ] [[Plantilla ARQUITECTURA.md]] ⭐
- [ ] [[Guía de estilo front GMM]] (Pendiente de confirmar: ¿es la misma que [[Guía GMM (original)]]?)
- [ ] [[Reglas de esquema SQL (tablas versionadas, procedimientos con CREATE OR ALTER)]] 🛠

---

## 02 · Fundamentos

### Internet y web
- [ ] [[Cliente y servidor]]
- [ ] [[Dirección IP y puertos]] (detalle en 19 · Redes)
- [ ] [[DNS]]
- [ ] [[TCP y UDP]]
- [ ] [[Anatomía de una URL]]
- [ ] [[HTTP vs HTTPS]]
- [ ] [[TLS y certificados]]
- [ ] [[HTTP 1.1, HTTP 2 y HTTP 3]]
- [ ] [[Proxy, proxy inverso y balanceador de carga]]
- [ ] [[CDN]]
- [ ] [[Latencia, ancho de banda y timeouts]]

### HTTP
- [ ] [[Verbos HTTP]] (nota índice)
- [ ] [[GET]]
- [ ] [[POST]]
- [ ] [[PUT]]
- [ ] [[PATCH]]
- [ ] [[DELETE]]
- [ ] [[HEAD y OPTIONS]]
- [ ] [[Métodos seguros e idempotentes]]
- [ ] [[Códigos de estado HTTP]] ⭐
- [ ] [[Headers HTTP]]
- [ ] [[Content-Type y Accept]]
- [ ] [[Parámetros de ruta, query string y body]]
- [ ] [[Caché HTTP (Cache-Control y ETag)]]
- [ ] [[Compresión HTTP]]
- [ ] [[Redirecciones]] 🛠 (301 de slugs en Dionisio)
- [ ] [[Subida de archivos (multipart)]] 🛠
- [ ] [[Paginación, filtrado y ordenamiento]] (offset vs cursor)
- [ ] [[Versionado de APIs]]
- [ ] [[Compatibilidad hacia atrás y cambios incompatibles en APIs]] ⭐
- [x] [[Idempotencia]] _(existe; incluye idempotency keys)_ 🛠
- [x] [[Rate limiting]] _(existe; concepto + implementación en ASP.NET Core)_ 🛠
- [x] [[Webhooks]] _(existe)_ 🛠
- [ ] [[Polling, long polling, SSE y WebSockets]]
- [x] [[SignalR]] _(existe; concepto + ASP.NET Core)_

### APIs
- [ ] [[REST]] ⭐
- [ ] [[Modelo de madurez de Richardson]]
- [ ] [[Diseño de endpoints y nombres de recursos]] ⭐
- [x] [[ProblemDetails]] _(existe)_ 🛠
- [x] [[OpenAPI con Swagger y Scalar]] _(existe)_ 🛠
- [ ] [[Contract-first y diseño de contratos]]
- [ ] [[gRPC]]
- [ ] [[GraphQL]]
- [ ] [[Mensajería y colas (conceptos)]] (detalle en 09 · Mensajería)

### Cookies
- [ ] [[Cookies]] (qué son y por qué existen)
- [ ] [[Set-Cookie y el header Cookie]]
- [ ] [[Cookies - Domain y Path]]
- [ ] [[Cookies - Expires y Max-Age]]
- [ ] [[Cookies - Secure y HttpOnly]] (incluye HttpOnly como mitigación de XSS)
- [ ] [[Cookies - SameSite]]
- [ ] [[Cookies de sesión y persistentes]]
- [ ] [[Cookies de primera y tercera parte]]
- [ ] [[Cookies entre sistemas propios (subdominios y dominios distintos)]]
- [ ] [[Mismo origen, mismo sitio y mismo dominio]]
- [ ] [[Cookies y CORS con credenciales]]
- [ ] [[Cookies y SSO]] 🛠 (sesión de Hecate)
- [ ] [[Cookies vs localStorage vs sessionStorage]] (complementa [[localStorage]])
- [ ] [[Cookies vs tokens]]
- [ ] [[Cookies y CSRF]]
- [ ] [[Cookies en ASP.NET Core (autenticación y Data Protection)]] 🛠
- [ ] [[Cookies desde JavaScript]]
- [ ] [[Restricciones a cookies de terceros en navegadores]]

### Autenticación y autorización
- [ ] [[Autenticación vs autorización]] ⭐
- [ ] [[Sesiones]]
- [ ] [[Tokens de acceso]]
- [x] [[JWT y autenticación con Bearer]] _(existe)_ 🛠
- [ ] [[Firmas de JWT (HS256 vs RS256) y JWKS]] ⭐🛠 (desajuste Hecate ↔ Dionisio)
- [ ] [[Rotación de llaves de firma]]
- [ ] [[Refresh tokens]]
- [ ] [[API keys]] 🛠 (X-Api-Key de Nomisma)
- [ ] [[OAuth 2.0]] ⭐
- [ ] [[OAuth 2.0 - Authorization Code con PKCE]] ⭐🛠
- [ ] [[OpenID Connect]] ⭐
- [ ] [[Google Sign-In]] 🛠
- [ ] [[SSO]]
- [ ] [[Roles, claims y policies]]
- [ ] [[MFA]]
- [ ] [[Hashing de contraseñas]] 🛠
- [ ] [[Flujo seguro de restablecer contraseña]] 🛠
- [ ] [[Microsoft Entra ID]]

### Datos y formatos
- [ ] [[JSON]]
- [ ] [[XML]]
- [ ] [[CSV]]
- [ ] [[Base64]]
- [ ] [[UTF-8 y codificaciones]] (incluye BOM y finales de línea CRLF/LF)
- [ ] [[Fechas, zonas horarias y UTC]] 🛠 (hora del centro de México en ReporteriaAzure)
- [ ] [[ISO 8601]]
- [ ] [[GUID y UUID]]
- [ ] [[Dinero y decimales]] 🛠
- [ ] [[Expresiones regulares]]
- [ ] [[Internacionalización y localización (es-MX)]]

### Git
- [ ] [[Git - qué es y cómo funciona]]
- [ ] [[Git - commit]]
- [ ] [[Git - ramas]]
- [ ] [[Git - merge]]
- [ ] [[Git - rebase]]
- [ ] [[Git - cherry-pick]]
- [ ] [[Git - stash]]
- [ ] [[Git - resolver conflictos]]
- [ ] [[Git - reset y revert]]
- [ ] [[Git - .gitignore]]
- [ ] [[Git - pull requests]]
- [ ] [[Estrategias de ramas (GitFlow y trunk-based)]]
- [ ] [[Conventional Commits]]
- [ ] [[Git - tags y versionado semántico]]
- [ ] [[Git - hooks y pre-commit]] (una sola nota; 08 enlaza aquí)

### Computación general
- [ ] [[Terminal (bash y PowerShell)]]
- [ ] [[Linux para desarrolladores (SSH, permisos, procesos y systemd)]]
- [ ] [[Variables de entorno]]
- [ ] [[Procesos e hilos]]
- [ ] [[Concurrencia vs paralelismo]]
- [ ] [[Síncrono vs asíncrono]]
- [ ] [[Estructuras de datos básicas]]
- [ ] [[Complejidad algorítmica (Big O)]] ⭐
- [ ] [[Algoritmos básicos (búsqueda, ordenamiento, recursión)]]
- [ ] [[Principios SOLID]] ⭐
- [ ] [[DRY, KISS y YAGNI]]
- [ ] [[Acoplamiento y cohesión]] ⭐
- [ ] [[Code smells y refactorización]]
- [ ] [[Clean code]]
- [ ] [[Markdown]]

---

## 03 · C# y .NET

### Básico
- [x] [[NET y CSharp]] _(existe; nota índice del área)_
- [ ] [[Qué es .NET, el CLR y el SDK]]
- [ ] [[Solución, proyecto y estructura de carpetas]] (incluye .slnx)
- [ ] [[Tipos de valor y de referencia]]
- [ ] [[Tipos primitivos]]
- [ ] [[string e inmutabilidad]]
- [ ] [[var y tipado implícito]]
- [ ] [[null y nullable reference types]]
- [ ] [[Nullable de valor (Nullable de int)]]
- [ ] [[Operadores de null en CSharp]]
- [ ] [[Condicionales y bucles]]
- [ ] [[Métodos y parámetros (ref, out, in, params)]]
- [ ] [[Parámetros opcionales y nombrados]]
- [ ] [[Arrays]]
- [ ] [[Colecciones (List, Dictionary, HashSet, Queue, Stack)]]
- [ ] [[Enums]]
- [ ] [[Excepciones (try, catch, finally, throw)]]
- [ ] [[using y namespaces]]
- [ ] [[Conversión de tipos (casting, parse, TryParse)]]
- [ ] [[DateTime vs DateTimeOffset]] 🛠

### Orientación a objetos
- [ ] [[Clases y objetos]]
- [ ] [[Propiedades, campos y modificadores de acceso]]
- [ ] [[Constructores]] (incluye primary constructors)
- [ ] [[Encapsulamiento]] 🛠 (setters privados y fábricas Rehydrate en Dionisio)
- [ ] [[Herencia]]
- [ ] [[Polimorfismo]]
- [ ] [[Clases abstractas]]
- [ ] [[Interfaces]]
- [ ] [[Interfaz vs clase abstracta]]
- [ ] [[Programar contra interfaces]]
- [ ] [[Implementación explícita de interfaces]]
- [ ] [[Implementaciones por defecto en interfaces]]
- [ ] [[Clases estáticas y miembros estáticos]]
- [ ] [[sealed y override]]
- [ ] [[Clases parciales]]
- [ ] [[struct]]
- [ ] [[record]]
- [ ] [[record struct]]
- [ ] [[init y required]]
- [ ] [[Igualdad por valor vs por referencia]]
- [ ] [[Tuplas y deconstrucción]]
- [ ] [[Clases anidadas y DTOs]]

### Intermedio
- [ ] [[Genéricos]]
- [ ] [[Constraints de genéricos]]
- [ ] [[Varianza (in y out) en genéricos]] 🛠
- [ ] [[Delegates]]
- [ ] [[Events]]
- [ ] [[Func, Action y Predicate]]
- [ ] [[Expresiones lambda]]
- [ ] [[Métodos de extensión]]
- [ ] [[Pattern matching]]
- [ ] [[switch expressions]]
- [ ] [[IEnumerable, ICollection e IList]]
- [ ] [[IEnumerable vs IQueryable]]
- [ ] [[LINQ]]
- [ ] [[Ejecución diferida en LINQ]]
- [ ] [[yield]]
- [ ] [[IAsyncEnumerable]]
- [ ] [[Indexadores]]
- [ ] [[Sobrecarga de operadores]]
- [ ] [[Atributos]]
- [ ] [[Reflexión]] 🛠 (registro automático de sentencias SQL)
- [ ] [[Recursos embebidos (EmbeddedResource)]] 🛠
- [ ] [[Source generators y GeneratedRegex]] 🛠
- [ ] [[nameof y otras utilidades del lenguaje]]
- [ ] [[Novedades recientes de CSharp (collection expressions, primary constructors y más)]]
- [ ] [[StringBuilder]]
- [ ] [[IDisposable y using]]
- [ ] [[Recolector de basura]]
- [ ] [[Manejo de archivos y streams]]
- [ ] [[Serialización con System.Text.Json]]

### Asincronía y concurrencia
- [ ] [[Task y async-await]] ⭐
- [ ] [[Cómo funciona async por dentro (máquina de estados)]]
- [ ] [[ValueTask]]
- [ ] [[CancellationToken]] ⭐🛠 (CancellationToken.None tras efectos de la pasarela)
- [ ] [[ConfigureAwait]]
- [ ] [[async void y por qué evitarlo]]
- [ ] [[Sync over async y thread pool starvation]] ⭐
- [ ] [[Task.WhenAll y Task.WhenAny]]
- [ ] [[Hilos y ThreadPool]]
- [ ] [[lock y concurrencia]]
- [ ] [[Parallel y PLINQ]]
- [x] [[System.Threading.Channels]] _(existe)_
- [ ] [[Span y Memory]]

### ASP.NET Core
- [ ] [[Pipeline y middleware]] ⭐
- [ ] [[Generic host y Kestrel]]
- [ ] [[Routing]]
- [x] [[ASP.NET Core Controllers]] _(existe)_ 🛠
- [x] [[ASP.NET Core Minimal APIs]] _(existe; agregar comparación con Controllers)_
- [ ] [[Model binding y validación]]
- [ ] [[Filtros]] 🛠 (ValidacionFiltro de Hecate)
- [x] [[Inyección de dependencias]] _(existe)_ ⭐
- [ ] [[Ciclos de vida (Singleton, Scoped, Transient)]] ⭐
- [ ] [[Keyed services]]
- [ ] [[Configuración y appsettings]]
- [x] [[Options pattern]] _(existe; agregar ValidateOnStart)_ 🛠
- [x] [[Secretos con User Secrets]] _(existe; nota hija de [[Manejo de secretos]])_
- [ ] [[Logging con ILogger]]
- [ ] [[Serilog]]
- [ ] [[Manejo global de errores (IExceptionHandler)]] 🛠
- [ ] [[CORS en ASP.NET Core]] (configuración segura en 12 · Seguridad)
- [ ] [[Autenticación con JWT en ASP.NET Core]] 🛠
- [ ] [[Autorización con policies]]
- [ ] [[Data Protection]] 🛠
- [ ] [[Versionado de API en ASP.NET Core]]
- [ ] [[Microsoft.AspNetCore.OpenApi vs Swashbuckle]] (complementa [[OpenAPI con Swagger y Scalar]])
- [ ] [[Health checks]]
- [x] [[Output caching]] _(existe)_
- [ ] [[Caché en memoria, distribuida y HybridCache]]
- [x] [[BackgroundService]] _(existe)_
- [ ] [[HttpClient e IHttpClientFactory]] ⭐ (incluye agotamiento de sockets)
- [x] [[Flurl]] _(existe)_ 🛠
- [ ] [[Resiliencia con Polly]]
- [ ] [[MediatR]] 🛠 (licencia; complementa [[CQRS y mediador]])
- [x] [[FluentValidation]] _(existe)_ 🛠
- [ ] [[AutoMapper y alternativas]] (tu regla: mapeo a mano)
- [ ] [[Subida y descarga de archivos]]
- [ ] [[Publicación y despliegue de .NET]]
- [ ] [[Publicar en IIS (FTP y FileZilla)]] 🛠 (ReporteriaAzure)
- [ ] [[Dockerfile para .NET]] 🛠
- [ ] [[Native AOT y rendimiento]]
- [ ] [[Directory.Build.props y Central Package Management]] 🛠
- [ ] [[Analizadores de Roslyn y warnings como errores]]
- [ ] [[Crear y publicar paquetes NuGet]]

### Rendimiento en .NET
- [ ] [[BenchmarkDotNet]] ⭐
- [ ] [[dotnet-counters, dotnet-trace y dotnet-dump]] ⭐
- [ ] [[Allocations y modos del GC]]
- [ ] [[JIT y tiered compilation]]

### Acceso a datos y ORMs
- [ ] [[Qué es un ORM y cuándo usarlo]]
- [ ] [[ADO.NET]]
- [x] [[Dapper]] _(existe)_ 🛠
- [ ] [[Dapper - QueryMultiple y multi-mapping]]
- [ ] [[Dapper - transacciones]] 🛠 (SesionSql de Dionisio)
- [ ] [[Dapper - table-valued parameters]]
- [x] [[SQL en archivos con una interfaz por consulta]] _(existe)_ ⭐🛠
- [x] [[Entity Framework Core]] _(existe; marcar que tus proyectos ya no lo usan)_
- [ ] [[EF Core - DbContext]]
- [ ] [[EF Core - migraciones]]
- [ ] [[EF Core - tracking vs no tracking]]
- [ ] [[EF Core - relaciones]]
- [ ] [[EF Core - carga eager, lazy y explícita]]
- [ ] [[Problema N+1]] ⭐
- [ ] [[EF Core - SQL crudo]]
- [ ] [[Dapper vs EF Core vs ADO.NET]] ⭐
- [x] [[Repository]] _(existe; agregar Unit of Work)_ 🛠
- [ ] [[Connection pooling]]
- [ ] [[Parametrización y SQL injection]] ⭐
- [x] [[Patrón Outbox]] _(existe)_
- [x] [[SQLite]] _(existe)_

### Pruebas en .NET
- [x] [[xUnit]] _(existe)_ 🛠
- [ ] [[Mocks, stubs y fakes]] 🛠
- [ ] [[Moq y NSubstitute]]
- [ ] [[Pruebas de integración con WebApplicationFactory]] ⭐
- [ ] [[Testcontainers]]
- [ ] [[TimeProvider para probar fechas]]
- [ ] [[Pruebas de reglas de arquitectura (NetArchTest)]] 🛠 (como SentenciasSqlTests)

---

## 04 · JavaScript y Web

### HTML y CSS
- [ ] [[HTML semántico]]
- [ ] [[Formularios HTML]]
- [x] [[Elemento dialog]] _(existe)_
- [ ] [[Accesibilidad web]] ⭐
- [ ] [[CSS - selectores y especificidad]]
- [ ] [[CSS - box model]]
- [ ] [[CSS - flexbox]]
- [ ] [[CSS - grid]]
- [ ] [[CSS - diseño responsivo]]
- [x] [[Imágenes responsive]] _(existe)_ 🛠
- [ ] [[CSS - variables]] (temas claro/oscuro)
- [ ] [[CSS - animaciones y transiciones]]
- [ ] [[CSS - metodologías (BEM)]]

### Básico de JavaScript
- [ ] [[Cómo se ejecuta JavaScript (navegador y Node)]]
- [ ] [[var, let y const]]
- [ ] [[Tipos de datos en JS]]
- [ ] [[Coerción de tipos]]
- [ ] [[== vs ===]]
- [ ] [[Valores truthy y falsy]]
- [ ] [[Operadores de null y lógicos en JS]]
- [ ] [[Condicionales y bucles en JS]]
- [ ] [[Funciones (declaración, expresión y flecha)]]
- [ ] [[Alcance (scope)]]
- [ ] [[Hoisting]]
- [ ] [[Closures]] ⭐
- [ ] [[this en JavaScript]]
- [ ] [[call, apply y bind]]
- [ ] [[Objetos]]
- [ ] [[Prototipos y herencia prototípica]]
- [ ] [[Clases en JS]]
- [ ] [[Campos privados en clases JS]]
- [ ] [[Destructuring]]
- [ ] [[Spread y rest]]
- [ ] [[Template literals]]
- [ ] [[Arrays y sus métodos]]
- [ ] [[map, filter y reduce]]
- [ ] [[find, some, every y flatMap]]
- [ ] [[sort]]
- [ ] [[Map y Set]]
- [ ] [[WeakMap y WeakSet]]
- [ ] [[JSON.stringify y JSON.parse]]
- [ ] [[Fechas en JS]]
- [ ] [[Intl (formato de números y fechas)]]
- [x] [[ES Modules e import maps]] _(existe)_ 🛠
- [ ] [[Manejo de errores en JS]]
- [ ] [[structuredClone y copia profunda]]

### Asincronía en JS
- [ ] [[Callbacks]]
- [ ] [[Promesas]]
- [ ] [[async y await]]
- [ ] [[Promise.all, allSettled y race]]
- [ ] [[Event loop]] ⭐
- [ ] [[Microtareas y macrotareas]]
- [ ] [[Timers]]
- [ ] [[fetch]]
- [x] [[AbortController]] _(existe)_
- [ ] [[Generadores e iteradores]]

### El navegador
- [ ] [[DOM - selección y creación]]
- [ ] [[DOM - eventos]]
- [ ] [[Propagación de eventos (bubbling y capturing)]]
- [ ] [[Delegación de eventos]]
- [ ] [[Formularios y FormData]]
- [x] [[localStorage]] _(existe; agregar sessionStorage)_
- [ ] [[History API y navegación]]
- [x] [[SPA y router por hash]] _(existe)_ 🛠
- [x] [[postMessage e iframes]] _(existe)_ 🛠
- [ ] [[IntersectionObserver y MutationObserver]]
- [ ] [[Web Components]]
- [ ] [[Shadow DOM]]
- [ ] [[Service Workers y PWA]]
- [ ] [[Seguridad del DOM (innerHTML y escape de datos)]]
- [ ] [[Pipeline de renderizado del navegador]]

### Front sin bundler (tu variante de reglas)
- [x] [[Molde Service-View-Handlers-Events]] _(existe)_ 🛠
- [x] [[lit-html]] _(existe)_ 🛠

### Avanzado
- [ ] [[Proxy y Reflect]]
- [ ] [[Symbol]]
- [ ] [[Debounce y throttle]]
- [ ] [[Fugas de memoria]]
- [ ] [[Rendimiento web y Core Web Vitals]] ⭐
- [ ] [[SEO y Open Graph]] 🛠 (portfolios de Itzamna)
- [ ] [[Depuración con DevTools]]
- [ ] [[npm y package.json]]
- [x] [[pnpm workspaces]] _(existe; monorepos)_ 🛠
- [x] [[Vite]] _(existe)_ 🛠
- [ ] [[ESLint y Prettier]]
- [x] [[TypeScript]] _(existe; nota índice)_ 🛠
- [ ] [[TypeScript - tipos básicos]]
- [ ] [[TypeScript - interfaces y types]]
- [ ] [[TypeScript - genéricos]]
- [ ] [[TypeScript - uniones y narrowing]]
- [ ] [[Node.js básico]]
- [ ] [[Pruebas con Vitest y Jest]]
- [ ] [[Librería de componentes propia en JS plano]]
- [ ] [[Design systems]]
- [ ] [[Micro-frontends (conceptos)]]

---

## 05 · React

- [x] [[React]] _(existe; nota índice)_ 🛠
- [ ] [[Qué es React y por qué existe]]
- [ ] [[JSX]]
- [ ] [[Componentes]]
- [ ] [[Props]]
- [ ] [[Estado con useState]]
- [ ] [[Eventos en React]]
- [ ] [[Listas y keys]]
- [ ] [[Renderizado condicional]]
- [x] [[Componentes controlados]] _(existe; formularios controlados)_
- [ ] [[useEffect]] ⭐
- [ ] [[useRef]]
- [ ] [[useMemo y useCallback]]
- [ ] [[useReducer]]
- [ ] [[Context]]
- [ ] [[Custom hooks]] 🛠 (useDraftForm, useEntityForm)
- [x] [[React Router]] _(existe; data router)_ 🛠
- [ ] [[Rutas protegidas]]
- [x] [[TanStack Query]] _(existe)_ 🛠
- [x] [[zustand]] _(existe; agregar comparación con Redux)_ 🛠
- [x] [[dnd-kit]] _(existe)_ 🛠
- [x] [[react-easy-crop]] _(existe)_ 🛠
- [ ] [[Suspense y lazy loading]]
- [ ] [[Error boundaries]]
- [ ] [[Rendimiento en React]]
- [ ] [[Pruebas con Testing Library]]
- [ ] [[Estilos (CSS Modules y Tailwind)]]
- [ ] [[Estructura de carpetas por feature]]
- [ ] [[Next.js (SSR, SSG y App Router)]] ⭐
- [ ] [[Server Components]]
- [ ] [[Máquinas de estado en UI (XState)]]

---

## 06 · SQL Server

### Básico
- [x] [[SQL Server]] _(existe; nota índice)_ 🛠
- [ ] [[Modelo relacional]]
- [ ] [[Tipos de datos (varchar vs nvarchar, decimal vs float)]]
- [ ] [[datetime2 y fechas en SQL Server]] 🛠
- [ ] [[uniqueidentifier]]
- [ ] [[Esquemas (schemas) en SQL Server]] 🛠 (portfolio, dbo)
- [ ] [[CREATE, ALTER y DROP (DDL)]]
- [ ] [[INSERT, UPDATE y DELETE (DML)]]
- [ ] [[SELECT]]
- [ ] [[WHERE, LIKE, IN y BETWEEN]]
- [ ] [[ORDER BY, TOP y OFFSET FETCH]]
- [ ] [[DISTINCT]]
- [ ] [[NULL y lógica de tres valores]]
- [ ] [[Funciones de texto]]
- [ ] [[Funciones de fecha]]
- [ ] [[CASE]]
- [ ] [[Funciones de agregación]]
- [ ] [[GROUP BY y HAVING]]
- [ ] [[JOINs]] ⭐
- [ ] [[INNER JOIN]]
- [ ] [[LEFT y RIGHT JOIN]]
- [ ] [[FULL JOIN y CROSS JOIN]]
- [ ] [[Self join]]
- [ ] [[UNION, UNION ALL, INTERSECT y EXCEPT]]
- [ ] [[Claves primarias y foráneas]]
- [ ] [[UNIQUE, CHECK y DEFAULT]]
- [ ] [[IDENTITY vs SEQUENCE]]
- [ ] [[Normalización]] ⭐

### Intermedio
- [ ] [[Subconsultas]]
- [ ] [[Subconsultas correlacionadas]]
- [ ] [[EXISTS vs IN]]
- [ ] [[CTE]]
- [ ] [[CTE múltiples]]
- [ ] [[CTE recursivas]]
- [ ] [[CROSS APPLY y OUTER APPLY]]
- [ ] [[Funciones de ventana]] ⭐
- [ ] [[ROW_NUMBER, RANK y DENSE_RANK]]
- [ ] [[LAG y LEAD]]
- [ ] [[Totales acumulados con OVER]]
- [ ] [[PIVOT y UNPIVOT]]
- [ ] [[STRING_AGG y STRING_SPLIT]]
- [ ] [[JSON en SQL Server (OPENJSON y FOR JSON)]] 🛠
- [ ] [[Nivel de compatibilidad de la base]] 🛠
- [ ] [[XML en SQL Server]]
- [ ] [[MERGE]]
- [ ] [[OUTPUT]]
- [ ] [[Tablas temporales y variables de tabla]]
- [ ] [[Table-valued parameters]]
- [ ] [[Vistas]]
- [ ] [[Funciones escalares y de tabla]]
- [ ] [[Stored procedures]] ⭐🛠
- [ ] [[CREATE OR ALTER y scripts idempotentes]] 🛠
- [ ] [[Lotes con GO, sqlcmd e Invoke-Sqlcmd]] 🛠
- [ ] [[Parámetros de salida y RETURN]]
- [ ] [[SET NOCOUNT y SET XACT_ABORT]] 🛠
- [ ] [[TRY CATCH y THROW]] 🛠 (error 50409 de concurrencia)
- [ ] [[Triggers]]
- [ ] [[Cursores y por qué evitarlos]]
- [ ] [[SQL dinámico y sp_executesql]]
- [ ] [[Paginación eficiente]]
- [x] [[Borrado lógico y auditoría]] _(existe)_ 🛠

### Transacciones y concurrencia
- [ ] [[Propiedades ACID]] ⭐
- [ ] [[Transacciones (BEGIN, COMMIT y ROLLBACK)]]
- [ ] [[Transacciones anidadas y savepoints]]
- [ ] [[Niveles de aislamiento]] ⭐
- [ ] [[Bloqueos (locks)]]
- [ ] [[Deadlocks]]
- [ ] [[Snapshot y RCSI]]
- [x] [[Concurrencia optimista]] _(existe; con rowversion, EF y SP)_ 🛠

### Rendimiento
- [ ] [[Índices]] ⭐
- [ ] [[Índice clustered]]
- [ ] [[Índice nonclustered]]
- [ ] [[Columnas incluidas y filtered indexes]]
- [ ] [[Columnstore]]
- [ ] [[Planes de ejecución]] ⭐
- [ ] [[Estadísticas]]
- [ ] [[SARGability]]
- [ ] [[Parameter sniffing]]
- [ ] [[Query Store]]
- [ ] [[DMVs]]
- [ ] [[Extended Events]]

### Administración y avanzado
- [ ] [[Tablas temporales del sistema (temporal tables)]]
- [ ] [[Particionamiento]]
- [ ] [[Roles y permisos]] 🛠 (GRANT EXECUTE ON SCHEMA)
- [ ] [[Cifrado y Always Encrypted]]
- [ ] [[Row-level security]]
- [ ] [[Backup y restore]]
- [ ] [[Modelos de recuperación]]
- [ ] [[SQL Server Agent y jobs]]
- [ ] [[Linked servers]]
- [ ] [[Collation]]
- [ ] [[Auditoría (CDC y Change Tracking)]]
- [ ] [[Migraciones de base de datos (scripts versionados, DACPAC, DbUp y Flyway)]] ⭐🛠 (una sola nota; 08 enlaza aquí)
- [ ] [[Migraciones sin caídas (expand y contract)]] ⭐
- [x] [[Multi-tenancy]] _(existe; diseño en base de datos + arquitectura)_ 🛠
- [ ] [[Azure SQL - diferencias con SQL Server]] 🛠
- [ ] [[Azure SQL serverless (auto-pausa y reintentos)]] 🛠
- [x] [[PostgreSQL]] _(existe; agregar "para quien viene de SQL Server")_ 🛠
- [ ] [[Funciones y procedimientos en PostgreSQL]] 🛠 (payment.sp_* de Nomisma)
- [x] [[Snapshot inmutable versionado]] _(existe)_ 🛠

### Otros datos
- [ ] [[Redis más allá de la caché (locks, colas y contadores)]]
- [ ] [[Búsqueda de texto completo]]
- [ ] [[Modelado en NoSQL]]
- [ ] [[ETL (conceptos)]]

---

## 07 · Azure

Cada nota de servicio lleva: qué es, cuándo usarlo, alternativas, costo y free tier, enlace con la guía de proyectos.

### Fundamentos
- [ ] [[Azure - suscripciones, grupos de recursos y regiones]]
- [ ] [[Azure - Resource Manager]]
- [ ] [[Azure - etiquetas (tags)]]
- [ ] [[Azure - costos y Cost Management]]
- [ ] [[Azure - capas gratuitas]]
- [ ] [[Azure - RBAC]]
- [ ] [[Azure - Entra ID y registro de aplicaciones]]
- [ ] [[Azure CLI]]
- [ ] [[Azure Portal vs CLI vs IaC]]
- [ ] [[Azure Well-Architected Framework]] ⭐
- Certificaciones: ver [[Mapa de certificaciones de Azure y cómo elegir]] (sección 18)

### Dónde probar Azure sin pagar
- [ ] [[Cuenta gratuita de Azure (crédito de 30 días, servicios gratuitos y límite de gasto)]]
- [ ] [[Servicios de Azure siempre gratuitos]]
- [ ] [[Servicios de Azure gratuitos durante 12 meses]]
- [ ] [[Probar servicios de pago con el crédito de la cuenta gratuita]]
- [ ] [[Azure for Students]]
- [ ] [[Microsoft Learn sandbox (suscripción temporal sin tarjeta)]]
- [ ] [[Créditos de Azure para suscriptores de Visual Studio]]
- [ ] [[Pay as you go y cuándo se pierde la protección de gasto]]
- [ ] [[Cómo evitar cobros inesperados en la nube (presupuestos y alertas)]] (una sola nota; 16 enlaza aquí)
- [ ] [[Cómo borrar recursos al terminar una práctica]]
- [ ] [[Crédito promocional vs proyectos con costo cero permanente]]

### Cómputo
- [ ] [[App Service]] 🛠
- [ ] [[App Service - planes y escalado]]
- [ ] [[App Service - deployment slots]]
- [ ] [[Azure Functions]]
- [x] [[Azure Static Web Apps]] _(existe)_ 🛠
- [ ] [[Container Apps]]
- [ ] [[Container Registry]]
- [ ] [[AKS (conceptos)]]
- [ ] [[Máquinas virtuales]]

### Datos
- [ ] [[Azure SQL Database]] 🛠
- [ ] [[Cosmos DB]]
- [x] [[Azure Blob Storage]] _(existe)_ 🛠
- [ ] [[Queue Storage y Table Storage]]
- [ ] [[Azure Cache for Redis]]
- [ ] [[Service Bus]]
- [ ] [[Event Grid]]
- [ ] [[Event Hubs]]
- [ ] [[Azure AI Search]]

### Integración
- [ ] [[API Management]]
- [x] [[Azure DevOps REST API]] _(existe)_ 🛠

### Seguridad
- [ ] [[Key Vault]] ⭐
- [ ] [[Managed Identity]] ⭐
- [ ] [[Microsoft Defender (conceptos)]]

### Redes en Azure
- [ ] [[Virtual Network]]
- [ ] [[Private Endpoints]]
- [ ] [[Front Door]]
- [ ] [[Application Gateway]]
- [ ] [[Dominios personalizados y certificados]]

### Observabilidad
- [ ] [[Application Insights]]
- [ ] [[Log Analytics]]
- [ ] [[Azure Monitor y alertas]]

### Azure DevOps
- [ ] [[Azure DevOps - organización y proyectos]]
- [ ] [[Azure Repos]]
- [x] [[Azure Pipelines]] _(existe; nota índice YAML)_ 🛠
- [ ] [[Azure Pipelines - agentes]]
- [ ] [[Azure Pipelines - service connections]]
- [ ] [[Azure Pipelines - variable groups]]
- [ ] [[Azure Pipelines - environments y aprobaciones]]
- [ ] [[Azure Pipelines - plantillas]]
- [ ] [[Azure Boards]] 🛠 (ReporteriaAzure lee Work Items)
- [ ] [[Azure Artifacts]]
- [ ] [[Azure Test Plans]]

### Infraestructura como código
- [ ] [[ARM templates]]
- [ ] [[Bicep]]
- [ ] [[Terraform]]
- [ ] [[PowerShell para Azure]]

---

## 08 · Automatización, DevOps y operación

### Automatización y CI/CD
- [ ] [[CI y CD - conceptos]] ⭐
- [ ] [[Alternativas para automatizar (comparativa)]]
- [ ] [[GitHub Actions]]
- [ ] [[Jenkins (conceptos)]]
- [ ] [[Infraestructura como código - conceptos]]
- [ ] [[Pulumi]]
- [ ] [[Scripts con PowerShell]]
- [ ] [[Scripts con bash]]
- [ ] [[Scripts con Python]]
- [ ] [[Docker]] ⭐
- [ ] [[Docker Compose]]
- [ ] [[Kubernetes - conceptos básicos]]
- [ ] [[Azure Functions programadas]]
- [ ] [[Logic Apps]]
- [ ] [[Power Automate]]
- [ ] [[n8n]]
- [ ] [[Zapier y Make]]
- [ ] [[cron y Programador de tareas]]
- Hooks de Git: ver [[Git - hooks y pre-commit]]
- [ ] [[Dependabot y Renovate]]
- [ ] [[Estrategias de despliegue (blue-green y canary)]]
- [ ] [[Rollback y gestión de releases]] ⭐
- [ ] [[Feature flags]]
- Migraciones: ver [[Migraciones de base de datos (scripts versionados, DACPAC, DbUp y Flyway)]]
- [ ] [[Análisis estático (SonarQube y linters)]]
- Pruebas E2E: ver [[Pruebas end-to-end (Playwright y Cypress)]] (sección 13)
- [ ] [[Automatización con agentes de IA]] (enlaza con 21)
- Secretos en pipelines: ver [[Manejo de secretos]]
- [ ] [[GitOps]]
- [ ] [[Métricas DORA]] ⭐

### Operación en producción (SRE)
- [ ] [[SLI, SLO y SLA]] ⭐
- [ ] [[Error budgets]]
- [ ] [[Manejo de incidentes]] ⭐
- [ ] [[Postmortems sin culpables]] ⭐
- [ ] [[Runbooks]]
- [ ] [[Guardias (on-call)]]
- [ ] [[Recuperación ante desastres (RPO y RTO)]] ⭐
- [ ] [[Multi-región y alta disponibilidad]]
- [ ] [[Chaos engineering (conceptos)]]
- [ ] [[Pruebas de carga (k6 y NBomber)]] ⭐

---

## 09 · Arquitectura y patrones

### Estilos de arquitectura
- [ ] [[Arquitectura en capas]]
- [x] [[Vertical Slice Architecture]] _(existe)_ 🛠
- [x] [[Arquitectura hexagonal]] _(existe)_
- [x] [[Clean Architecture]] _(existe)_ ⭐🛠
- [ ] [[Monolito modular]] ⭐
- [ ] [[Microservicios]] ⭐
- [ ] [[Arquitectura orientada a eventos]]
- [x] [[CQRS y mediador]] _(existe)_ 🛠
- [ ] [[Event Sourcing]]
- [ ] [[Domain-Driven Design - conceptos]] ⭐
- [ ] [[Entidades, value objects y agregados]]
- [ ] [[Bounded contexts]]
- [x] [[Eventos de dominio y reacciones]] _(existe)_ 🛠
- [ ] [[Arquitectura evolutiva]]
- [ ] [[Atributos de calidad y trade-offs]] ⭐

### Patrones de diseño
- [x] [[Patrón Strategy]] _(existe)_ 🛠
- [ ] [[Patrón Factory]]
- [ ] [[Patrón Builder]]
- [ ] [[Patrón Mediator]]
- [ ] [[Patrón Template Method]]
- [ ] [[Patrón Specification]]
- [ ] [[Patrón Observer]]
- [ ] [[Patrón Decorator]]
- [ ] [[Patrón Adapter]]
- [ ] [[Patrón Singleton]]
- [ ] [[Result pattern vs excepciones]] ⭐🛠
- [ ] [[Guard clauses]] 🛠

### Patrones de nube y sistemas distribuidos
- [ ] [[Patrón Saga]]
- [ ] [[Circuit breaker y reintentos]] ⭐
- [ ] [[Bulkhead y throttling]]
- [ ] [[Cache-aside y estrategias de caché]]
- [ ] [[Competing consumers]]
- [ ] [[Valet key (URLs firmadas)]] 🛠 (R2 y Blob)
- [ ] [[Strangler fig]] ⭐
- [ ] [[Anti-corruption layer]]
- [ ] [[Backend for Frontend (BFF)]]
- [ ] [[Sidecar]]
- [ ] [[Consistencia eventual y teorema CAP]] ⭐
- [ ] [[Modelos de consistencia (fuerte, eventual, read-your-writes)]]
- Idempotencia distribuida: ver [[Idempotencia]]
- [x] [[Multi-tenancy]] _(existe)_
- [ ] [[Escalabilidad horizontal y vertical]] ⭐
- [ ] [[Las doce factores (12-factor)]]

### Mensajería
- [ ] [[Garantías de entrega (at-most, at-least y exactly-once)]] ⭐
- [ ] [[Dead letter queues y mensajes venenosos]]
- [ ] [[Reintentos con backoff]]
- [ ] [[Consumidores idempotentes]] ⭐
- [ ] [[RabbitMQ]]
- [ ] [[Kafka]]

### Diseño de sistemas
- [ ] [[Diseño de sistemas - cómo abordarlo]] ⭐
- [ ] [[Requisitos funcionales y no funcionales]] ⭐
- [ ] [[Estimaciones rápidas (carga, almacenamiento y costo)]] ⭐
- [ ] [[Replicación y réplicas de lectura]]
- [ ] [[Sharding y particionamiento]]
- [ ] [[Locks distribuidos y relojes]]
- [ ] [[Backpressure]]
- [ ] [[Caso - acortador de URLs]]
- [ ] [[Caso - rate limiter]]
- [ ] [[Caso - sistema de notificaciones]]
- [ ] [[Caso - feed de noticias]]
- [ ] [[Caso - sistema de pagos]] 🛠
- [ ] [[Caso - chat en tiempo real]]
- [ ] [[Caso - subida y procesamiento de archivos]] 🛠

### Documentar decisiones
- [ ] [[ADR - registro de decisiones de arquitectura]] ⭐🛠 (ARQUITECTURA.md)
- [ ] [[Design docs y RFCs]] ⭐
- [ ] [[Diagramas C4]]
- [ ] [[Diagramas de secuencia]]
- [ ] [[Gestión de deuda técnica]] ⭐
- [ ] [[Modernizar código legado]] ⭐
- [ ] [[Cómo elegir una arquitectura]]

---

## 10 · Radar de herramientas

Cada herramienta lleva estado (probar, evaluar, adoptar o descartar), motivo y fecha. Las herramientas nuevas se buscan en la web al momento de escribir la nota.

- [ ] [[Radar - cómo usarlo]]
- [ ] [[Radar - IA para desarrollo]]
- [ ] [[Radar - observabilidad]]
- [ ] [[Radar - infraestructura como código]]
- [ ] [[Radar - bases de datos]]
- [ ] [[Radar - testing]]
- [ ] [[Radar - experiencia de desarrollo]]
- [ ] [[Postman, Bruno y archivos .http]]
- [ ] [[SSMS, Azure Data Studio y DBeaver]]
- [ ] [[.NET Aspire]]
- [ ] [[OpenTelemetry]]
- [ ] [[Mermaid y diagramas como código]]
- [ ] [[Obsidian (frontmatter, Dataview y plugins útiles)]]

---

## 11 · Proyectos

- [ ] [[Plantilla de nota de proyecto]]
- Una nota por proyecto, enlazada con su `ARQUITECTURA.md`:
  - [x] [[Ocelotl.Hermes]] · [x] [[Ocelotl.Iris]] · [x] [[Itzamna.Dionisio]] · [x] [[Itzamna.Clio]] · [x] [[ReporteriaAzure]] · [x] [[Pochteca.Nomisma]]
  - [ ] [[Hecate]] · [ ] [[Argo]] (cuando se pidan)

---

## 12 · Seguridad

> Enfoque: entender cómo funciona cada ataque para saber defenderse. La práctica se hace solo en laboratorios o en sistemas propios (ver sección 16 y la ética en esta sección).

### Fundamentos de seguridad
- [ ] [[OWASP Top 10]] ⭐
- [ ] [[Mentalidad de seguridad y modelado de amenazas]] ⭐
- [ ] [[Tríada CIA (confidencialidad, integridad y disponibilidad)]]
- [ ] [[Superficie de ataque]]
- [ ] [[Defensa en profundidad]]
- [ ] [[Principio de mínimo privilegio]]
- [ ] [[Validación de entrada, codificación de salida y listas blancas]]
- [ ] [[Same-origin policy]]
- [ ] [[Cifrado simétrico y asimétrico]]
- [ ] [[Cifrado en reposo y en tránsito]]
- [ ] [[Firmas digitales]]
- [ ] [[Manejo de secretos]] ⭐🛠 (nota índice: user secrets, appsettings según publicación, Key Vault, pipelines, repositorios, rotación)
- [ ] [[Logs y datos sensibles]] 🛠 (limpieza de eventos en Sentry)
- [ ] [[Ciclo de desarrollo seguro (SSDLC)]] ⭐
- [ ] [[Seguridad de la cadena de suministro (SBOM y firmas)]]
- [ ] [[Privacidad y protección de datos (ley mexicana y GDPR)]] ⭐

### Inyección SQL
- [ ] [[SQL injection]] (nota índice)
- [ ] [[SQL injection - qué es y por qué ocurre]]
- [ ] [[SQL injection - concatenación de cadenas, la causa raíz]]
- [ ] [[SQL injection - cómo funciona un ataque (conceptual)]]
- [ ] [[SQL injection - tipos (basada en errores, UNION, ciega y de segundo orden)]]
- [ ] [[SQL injection - impacto real]]
- [ ] [[SQL injection - consultas parametrizadas]]
- [ ] [[SQL injection - Dapper y parámetros]] 🛠
- [ ] [[SQL injection - EF Core (FromSqlRaw vs FromSqlInterpolated)]]
- [ ] [[SQL injection - stored procedures y SQL dinámico seguro (sp_executesql y QUOTENAME)]]
- [ ] [[SQL injection - listas blancas para ORDER BY y nombres de columnas]]
- [ ] [[SQL injection - mínimo privilegio en la cuenta de base de datos]]
- [ ] [[SQL injection - manejo de errores sin filtrar detalles]]
- [ ] [[SQL injection - cómo detectarla (revisión de código, SAST y DAST)]]
- [ ] [[SQL injection - WAF como capa adicional]]
- [ ] [[Inyección NoSQL]]
- [ ] [[Inyección de comandos del sistema operativo]]
- [ ] [[Otras inyecciones (LDAP y XPath)]]

### XSS (Cross-Site Scripting)
- [ ] [[XSS]] (nota índice)
- [ ] [[XSS - qué es y por qué ocurre]]
- [ ] [[XSS - reflejado]]
- [ ] [[XSS - almacenado]]
- [ ] [[XSS - basado en DOM]]
- [ ] [[XSS - contextos de salida (HTML, atributo, JavaScript y URL)]]
- [ ] [[XSS - impacto real]]
- [ ] [[XSS - codificación de salida según el contexto]]
- [ ] [[XSS - textContent vs innerHTML]]
- [ ] [[XSS - escape de datos del API en templates de JS]] 🛠
- [ ] [[XSS - sanitización de HTML (DOMPurify)]]
- [ ] [[XSS - protección por defecto de React y dangerouslySetInnerHTML]]
- [ ] [[XSS - Razor y HtmlEncoder en ASP.NET Core]]
- [x] [[Content Security Policy]] _(existe)_ 🛠
- [ ] [[Trusted Types]]
- HttpOnly como mitigación: ver [[Cookies - Secure y HttpOnly]]
- [ ] [[XSS - cómo detectarlo]]

### Otras vulnerabilidades web
- [ ] [[CSRF]]
- [ ] [[CORS]] (configuración segura)
- [ ] [[Control de acceso roto e IDOR]] ⭐🛠 (filtro de tenant en cada SP)
- [ ] [[Autenticación rota]]
- [ ] [[Ataques a JWT y cómo evitarlos]] 🛠
- [ ] [[SSRF]]
- [ ] [[Path traversal]]
- [ ] [[Subida de archivos insegura]] 🛠
- [ ] [[Deserialización insegura]]
- [ ] [[XXE]]
- [ ] [[Clickjacking]] 🛠 (frame-ancestors de Hecate)
- [ ] [[postMessage seguro (validar origen)]] 🛠
- [ ] [[Open redirect]] 🛠 (redirect_uri permitidas)
- [ ] [[Exposición de datos sensibles]]
- [ ] [[Componentes y dependencias vulnerables]]
- [ ] [[Configuración insegura]]
- [ ] [[Secretos en repositorios]] (hija de [[Manejo de secretos]])
- [ ] [[Condiciones de carrera]]
- [ ] [[Seguridad de APIs]] ⭐

### Herramientas defensivas
- [ ] [[SAST, DAST y SCA]]
- [ ] [[Escaneo de dependencias y de secretos]]
- [ ] [[OWASP ZAP]]
- [ ] [[Burp Suite Community]]
- [ ] [[Pruebas de seguridad en el pipeline]]
- [ ] [[WAF]]

### Hacking ético
- [ ] [[Qué es el hacking ético]]
- [ ] [[Marco legal y autorización por escrito]]
- [ ] [[Alcance (scope) de una prueba de seguridad]]
- [ ] [[Qué nunca hacer]]
- [ ] [[Divulgación responsable]]
- [ ] [[Programas de bug bounty]]
- [ ] [[CVE y CVSS]]
- [ ] [[Cómo escribir un reporte de vulnerabilidad]]
- [ ] [[Metodología de pruebas web (OWASP WSTG)]]
- [ ] [[Pentesting vs red team]]

---

## 13 · Testing y calidad

- [ ] [[Pirámide de pruebas]] ⭐
- [ ] [[Pruebas unitarias]]
- [ ] [[Pruebas de integración]]
- [ ] [[Pruebas end-to-end (Playwright y Cypress)]] (una sola nota)
- [ ] [[Snapshot testing]]
- [ ] [[TDD]]
- [ ] [[BDD]]
- [ ] [[Cobertura de pruebas]]
- [ ] [[Qué probar y qué no]] ⭐
- [ ] [[Diseñar código fácil de probar]] ⭐
- [ ] [[Revisión de código]]
- [ ] [[Logging estructurado]]
- [ ] [[Métricas, trazas y logs]] ⭐
- [ ] [[Sentry]] 🛠 (incluye plan gratuito; 16 enlaza aquí)
- [ ] [[Perfilado de rendimiento]]
- [ ] [[Documentación técnica]]

---

## 14 · Carrera y liderazgo técnico

### Qué es ser senior
- [ ] [[Qué significa ser senior (alcance, autonomía, impacto y ownership)]] ⭐
- [ ] [[Escalera de carrera (junior, semi senior, senior, staff)]] ⭐
- [ ] [[Plan personal hacia senior]] ⭐

### Liderazgo y comunicación
- [ ] [[Cómo hacer un buen code review]]
- [ ] [[Cómo documentar una decisión técnica]] (enlaza [[Design docs y RFCs]])
- [ ] [[Cómo explicar un tema técnico]]
- [ ] [[Comunicación con personas no técnicas]] ⭐
- [ ] [[Influir sin autoridad]] ⭐
- [ ] [[Negociar alcance y plazos con negocio]] ⭐
- [ ] [[Priorizar y saber decir que no]] ⭐
- [ ] [[Pensamiento de producto]] ⭐
- [ ] [[Dar y recibir feedback]]
- [ ] [[Manejo de conflictos]]
- [ ] [[Estimación de tareas]]
- [ ] [[Mentoría a otros desarrolladores]]
- [ ] [[Presentaciones técnicas]]
- [ ] [[Escribir artículos técnicos]]

### Proceso de trabajo
- [ ] [[Scrum en la práctica]]
- [ ] [[Kanban]]
- [ ] [[Historias de usuario y criterios de aceptación]]
- [ ] [[Definition of done]]
- [ ] [[Levantamiento de requisitos]]

### Búsqueda de trabajo
- [ ] [[Portafolio y GitHub]]
- [ ] [[Preparación para entrevistas técnicas]]
- [ ] [[Entrevistas de comportamiento (método STAR)]] ⭐
- [ ] [[Entrevistas de diseño de sistemas]] ⭐
- [ ] [[Negociación salarial]]

### Inglés técnico ⭐
- [ ] [[Inglés técnico - plan de estudio]]
- [ ] [[Inglés técnico - leer documentación y RFCs]]
- [ ] [[Inglés técnico - escribir PRs, issues y design docs]]
- [ ] [[Inglés técnico - hablar en reuniones y entrevistas]]
- [ ] [[Vocabulario técnico en inglés]]

### Hábitos
- [ ] [[Diario de aprendizaje]]
- [ ] [[Práctica deliberada (katas de código y de diseño)]]
- [ ] [[Leer código de otros]]
- [ ] [[Contribuir a open source]]

---

## 15 · Java

- [ ] [[Java - JVM, JDK y JRE]]
- [ ] [[Java - sintaxis básica]]
- [ ] [[Java - clases, interfaces y records]]
- [ ] [[Java - colecciones y streams]]
- [ ] [[Java - Maven y Gradle]]
- [ ] [[Spring Boot]]
- [ ] [[JPA e Hibernate]]
- [ ] [[Java vs CSharp (comparativa)]]

---

## 16 · Laboratorios y práctica

> Todo lo de esta sección se practica solo en entornos hechos para eso o en sistemas propios. Las condiciones y la gratuidad de cada plataforma se verifican al escribir su nota.

### Laboratorios de seguridad
- [ ] [[PortSwigger Web Security Academy]]
- [ ] [[PortSwigger - ruta recomendada (Apprentice, Practitioner y Expert)]]
- [ ] [[PortSwigger - laboratorios de SQL injection]]
- [ ] [[PortSwigger - laboratorios de XSS]]
- [ ] [[OWASP Juice Shop]]
- [ ] [[DVWA]]
- [ ] [[OWASP WebGoat]]
- [ ] [[Hacker101]]
- [ ] [[TryHackMe]]
- [ ] [[Hack The Box]]
- [ ] [[PentesterLab]]
- [ ] [[picoCTF y retos CTF]]
- [ ] [[OverTheWire]]
- [ ] [[Cómo montar un laboratorio propio con Docker y red aislada]]
- [ ] [[Cómo llevar una bitácora de laboratorios resueltos]]

### Laboratorios de redes
- [ ] [[Cisco Packet Tracer]] (ver 19 · Redes)
- [ ] [[GNS3 y EVE-NG]]
- [ ] [[Laboratorio de redes en casa]]

### Laboratorios de IA y datos
- [ ] [[Google Colab]]
- [ ] [[Kaggle]]
- [ ] [[Hugging Face]]
- [ ] [[Modelos locales con Ollama]]

### Otras nubes (fuera de Azure) para probar servicios de pago
- [ ] [[AWS Free Tier - plan gratuito, créditos y servicios siempre gratuitos]]
- [ ] [[AWS - plan gratuito vs plan de pago (qué pasa cuando se acaban los créditos)]]
- [ ] [[Google Cloud - prueba gratuita (crédito por 90 días) y Free Tier]]
- [ ] [[Google Cloud Skills Boost (laboratorios con credenciales temporales)]]
- [ ] [[Google Cloud Shell]]
- [ ] [[Oracle Cloud - Always Free]]
- [ ] [[Comparativa de capas gratuitas de nubes (AWS, Azure, Google Cloud y Oracle)]]
- Cobros inesperados: ver [[Cómo evitar cobros inesperados en la nube (presupuestos y alertas)]]

### Laboratorios temporales en el navegador (sin cuenta de nube)
- [ ] [[Killercoda]]
- [ ] [[Play with Docker]]
- [ ] [[Play with Kubernetes]]
- [ ] [[GitHub Codespaces]]
- [ ] [[Learn Git Branching]]

### Plataformas de despliegue con plan gratuito
- [ ] [[Cloudflare (Pages y Workers)]]
- [x] [[Cloudflare R2]] _(existe)_
- [ ] [[Vercel]]
- [ ] [[Netlify]]
- [ ] [[Render]]
- [ ] [[Neon (PostgreSQL)]] 🛠
- [ ] [[Supabase]]
- [ ] [[Firebase]]

### Herramientas de pago con plan gratuito o prueba
- Sentry: ver [[Sentry]]
- [ ] [[Grafana Cloud - plan gratuito]]
- [ ] [[New Relic - plan gratuito]]
- [ ] [[Datadog - prueba gratuita]]
- [ ] [[SonarCloud]]
- [ ] [[Snyk - plan gratuito]]
- [ ] [[Postman - plan gratuito]]
- [ ] [[JetBrains - licencias para estudiantes y proyectos de código abierto]]
- [ ] [[Cómo aprovechar una prueba gratuita sin olvidar cancelarla]]

### Práctica de otras áreas
- [ ] [[Microsoft Learn - rutas gratuitas de Azure]]
- [ ] [[Retos de SQL (plataformas de práctica)]]
- [ ] [[Retos de JavaScript y CSharp (plataformas de práctica)]]
- [ ] [[Retos de diseño de sistemas]]
- [ ] [[Proyectos propios como laboratorio de aprendizaje]]

---

## 17 · Power BI

### Fundamentos
- [ ] [[Qué es la inteligencia de negocios (BI)]]
- [ ] [[Ecosistema de Power BI (Desktop, Service, Mobile, Report Server y Embedded)]]
- [ ] [[Power BI y Microsoft Fabric]]
- [ ] [[Licencias de Power BI (Free, Pro, Premium por usuario y capacidades)]]
- [ ] [[Instalar Power BI Desktop y primer informe]]
- [ ] [[Flujo de trabajo (obtener, transformar, modelar, visualizar y publicar)]]
- [ ] [[Mapa de lenguajes de Power BI (DAX, M, SQL, R, Python, Vega-Lite, TypeScript y JSON)]]
- [ ] [[Conectores y orígenes de datos (Excel, CSV, SQL Server, Web y API)]]
- [ ] [[Import, DirectQuery, Dual, Live Connection y Direct Lake]]
- [ ] [[Modelos compuestos]]

### Power Query y el lenguaje M
- [ ] [[Power Query - interfaz y pasos aplicados]]
- [ ] [[Power Query - tipos de datos]]
- [ ] [[Power Query - limpieza de datos]]
- [ ] [[Power Query - columnas condicionales y personalizadas]]
- [ ] [[Power Query - combinar consultas (merge y append)]]
- [ ] [[Power Query - agrupar, pivotar y despivotar]]
- [ ] [[Power Query - parámetros]]
- [ ] [[Power Query - funciones personalizadas]]
- [ ] [[Power Query - manejo de errores]]
- [ ] [[Power Query - consumir un API]] 🛠
- [ ] [[Power Query en Excel (consumir un API desde Excel)]] 🛠 (ReporteriaAzure)
- [ ] [[Lenguaje M - let e in]]
- [ ] [[Lenguaje M - tipos (listas, registros y tablas)]]
- [ ] [[Lenguaje M - funciones y recursividad]]
- [ ] [[Query folding]]
- [ ] [[Actualización incremental]]
- [ ] [[Dataflows]]
- [ ] [[Consultas nativas de SQL y vistas para Power BI]]

### Modelado de datos
- [ ] [[Modelo estrella (hechos y dimensiones)]]
- [ ] [[Granularidad de una tabla de hechos]]
- [ ] [[Relaciones (cardinalidad y dirección del filtro)]]
- [ ] [[Relaciones activas e inactivas]]
- [ ] [[Relaciones muchos a muchos y bidireccionales]]
- [ ] [[Dimensiones de rol múltiple]]
- [ ] [[Tabla de calendario]]
- [ ] [[Jerarquías]]
- [ ] [[Esquema copo de nieve vs estrella]]
- [ ] [[Seguridad a nivel de fila (RLS)]]
- [ ] [[Seguridad a nivel de objeto (OLS)]]
- [ ] [[Cómo funciona el motor VertiPaq]]

### DAX
- [ ] [[DAX - qué es y para qué sirve]]
- [ ] [[DAX - columnas calculadas, medidas y tablas calculadas]]
- [ ] [[DAX - sintaxis y operadores]]
- [ ] [[DAX - contexto de fila]]
- [ ] [[DAX - contexto de filtro]]
- [ ] [[DAX - transición de contexto]]
- [ ] [[DAX - CALCULATE]]
- [ ] [[DAX - FILTER, ALL, ALLEXCEPT y REMOVEFILTERS]]
- [ ] [[DAX - VALUES y DISTINCT]]
- [ ] [[DAX - iteradores (SUMX, AVERAGEX y demás)]]
- [ ] [[DAX - RELATED y RELATEDTABLE]]
- [ ] [[DAX - variables (VAR y RETURN)]]
- [ ] [[DAX - IF, SWITCH y DIVIDE]]
- [ ] [[DAX - conteos (COUNTROWS y DISTINCTCOUNT)]]
- [ ] [[DAX - inteligencia de tiempo]]
- [ ] [[DAX - YTD, mismo periodo del año anterior y comparativos]]
- [ ] [[DAX - RANKX y TOPN]]
- [ ] [[DAX - USERELATIONSHIP, TREATAS y CROSSFILTER]]
- [ ] [[DAX - SELECTEDVALUE, ISINSCOPE y HASONEVALUE]]
- [ ] [[DAX - tablas virtuales]]
- [ ] [[DAX - SUMMARIZE, SUMMARIZECOLUMNS y ADDCOLUMNS]]
- [ ] [[DAX - totales acumulados]]
- [ ] [[DAX - medidas semiaditivas]]
- [ ] [[DAX - patrones comunes (ABC, Pareto y cohortes)]]
- [ ] [[DAX - seguridad dinámica con USERPRINCIPALNAME]]
- [ ] [[DAX - errores comunes]]
- [ ] [[DAX - optimización]]
- [ ] [[Consultas DAX (EVALUATE, DEFINE y MEASURE)]]
- [ ] [[Grupos de cálculo]]
- [ ] [[Parámetros de campo]]
- [ ] [[Cálculos visuales (visual calculations)]]

### Ayudas para crear DAX más rápido
- [ ] [[Medidas rápidas (quick measures)]]
- [ ] [[Vista de consultas DAX (DAX query view)]]
- [ ] [[Copilot en la vista de consultas DAX (escribir y explicar consultas)]]
- [ ] [[Sugerencias de medidas rápidas con Copilot]]
- [ ] [[Requisitos y licencias para usar Copilot en Power BI]]
- [ ] [[Cuándo confiar en Copilot y cómo validar lo que genera]]
- [ ] [[DAX Studio]]
- [ ] [[Tabular Editor]]
- [ ] [[DAX Formatter y guías de estilo]]
- [ ] [[SQLBI y otros recursos para aprender DAX]]

### Visualización
- [ ] [[Tipos de gráficos y cuándo usar cada uno]]
- [ ] [[Principios de diseño de informes]]
- [ ] [[Colores y accesibilidad en informes]]
- [ ] [[Segmentaciones de datos (slicers)]]
- [ ] [[Filtros de visual, página e informe]]
- [ ] [[Interacciones entre visuales]]
- [ ] [[Información sobre herramientas (tooltips)]]
- [ ] [[Drill down y drill through]]
- [ ] [[Marcadores y botones]]
- [ ] [[Formato condicional]]
- [ ] [[Temas y archivos JSON de tema]]
- [ ] [[Diseño para móvil]]
- [ ] [[Informes paginados (Report Builder)]]
- [ ] [[Narrativa con datos (storytelling)]]

### Gráficas propias (visuales personalizados)
- [ ] [[Visuales personalizados - panorama de opciones]]
- [ ] [[Visuales de AppSource]]
- [ ] [[Visuales de R]]
- [ ] [[Visuales de Python]]
- [ ] [[Deneb (Vega y Vega-Lite)]]
- [ ] [[SVG dentro de medidas DAX]]
- [ ] [[SDK de visuales de Power BI (TypeScript y pbiviz)]]
- [ ] [[Crear un visual propio con D3]]
- [ ] [[Empaquetar, probar y publicar un visual propio]]

### Power BI Service
- [ ] [[Áreas de trabajo (workspaces)]]
- [ ] [[Modelos semánticos (antes datasets)]]
- [ ] [[Publicar y compartir informes]]
- [ ] [[Aplicaciones de Power BI]]
- [ ] [[Actualización programada]]
- [ ] [[Puerta de enlace de datos local (gateway)]]
- [ ] [[Paneles (dashboards) vs informes]]
- [ ] [[Suscripciones y alertas]]
- [ ] [[Seguridad y permisos en el Service]]
- [ ] [[Certificación y promoción de contenido]]
- [ ] [[Linaje de datos]]

### Avanzado
- [ ] [[Power BI Embedded (incrustar en una aplicación React o .NET)]]
- [ ] [[API REST de Power BI]]
- [ ] [[Cliente JavaScript de Power BI]]
- [ ] [[Punto de conexión XMLA]]
- [ ] [[TMDL y proyectos PBIP]]
- [ ] [[Power BI con Git y control de versiones]]
- [ ] [[Canalizaciones de implementación (deployment pipelines)]]
- [ ] [[ALM Toolkit]]
- [ ] [[Agregaciones]]
- [ ] [[Rendimiento - Performance Analyzer y VertiPaq Analyzer]]
- [ ] [[Modelos grandes]]
- [ ] [[Fabric - Lakehouse, Warehouse y OneLake]]
- [ ] [[Direct Lake]]
- [ ] [[Power BI con Azure (SQL, Synapse y Analysis Services)]]
- [ ] [[Power BI con Power Automate y Power Apps]]
- [ ] [[Gobierno de datos en Power BI]]

---

## 18 · Certificaciones

> Las certificaciones cambian de nombre y se retiran con frecuencia. Cada nota se verifica contra la página oficial al escribirla (nombres, códigos y fechas incluidos).

### Azure: ruta hasta arquitecto
- [ ] [[Mapa de certificaciones de Azure y cómo elegir]]
- [ ] [[AZ-900 Azure Fundamentals (opcional)]]
- [ ] [[AZ-104 Azure Administrator Associate]]
- [ ] [[AZ-204 Azure Developer Associate (retirado el 31 de julio de 2026)]] (fecha confirmada en la lista oficial de Microsoft)
- [ ] [[AI-200 Azure AI Cloud Developer Associate (sucesor de AZ-204)]] (Pendiente de confirmar)
- [ ] [[AZ-400 DevOps Engineer Expert]]
- [ ] [[AZ-305 Azure Solutions Architect Expert]] ⭐
- [ ] [[Requisitos y orden recomendado hasta Azure Solutions Architect]]
- [ ] [[AZ-500 y SC-500 (seguridad en Azure)]]
- [ ] [[DP-300 Azure Database Administrator]]
- [ ] [[SC-900 Security, Compliance and Identity Fundamentals]]
- [ ] [[SC-100 Cybersecurity Architect Expert]]
- [ ] [[Renovación anual de certificaciones de Microsoft]]
- [ ] [[Cómo estudiar con Microsoft Learn y evaluaciones de práctica]]
- [ ] [[Cómo prepararse para los casos de estudio de AZ-305]]

### IA y datos
- [ ] [[AI-900 Azure AI Fundamentals]] (el examen ahora es AI-901, ver la nota)
- [ ] [[AI-102 Azure AI Engineer Associate]] (examen retirado el 30 de junio de 2026; hoy es AI-103, ver la nota)
- [ ] [[Certificaciones de machine learning (Azure, AWS y Google)]]
- [ ] [[TensorFlow y PyTorch - certificados y cursos reconocidos]]

### Power BI y datos
- [ ] [[PL-300 Power BI Data Analyst Associate]]
- [ ] [[DP-600 Fabric Analytics Engineer Associate]]
- [ ] [[DP-900 Azure Data Fundamentals]]

### Redes (Cisco y otras)
- [ ] [[Cisco Networking Academy (cursos y Packet Tracer)]]
- [ ] [[Cisco CCST Networking]]
- [ ] [[CCNA]] ⭐ (si vas por redes)
- [ ] [[CCNP Enterprise]]
- [ ] [[CompTIA Network+]]

### DevOps, contenedores y nube en general
- [ ] [[GH-900 GitHub Foundations]]
- [ ] [[GH-200 GitHub Actions]]
- [ ] [[Terraform Associate]]
- [ ] [[Kubernetes (KCNA, CKAD y CKA)]]
- [ ] [[AWS Cloud Practitioner]]
- [ ] [[AWS Solutions Architect Associate]]
- [ ] [[Google Cloud Professional Cloud Architect]]

### Seguridad
- [ ] [[Burp Suite Certified Practitioner (PortSwigger)]]
- [ ] [[CompTIA Security+]]
- [ ] [[eJPT]]
- [ ] [[CEH]]
- [ ] [[OSCP]]
- [ ] [[Certified in Cybersecurity (ISC2)]]
- [ ] [[SC-200 Security Operations Analyst]]

### Desarrollo
- [ ] [[Certificaciones de CSharp y .NET]]
- [ ] [[Certificaciones de JavaScript y Node.js]]
- [ ] [[React - por qué no hay certificación oficial y qué hacer en su lugar]]
- [ ] [[Angular - certificaciones y cómo demostrar el nivel]]
- [ ] [[Certificaciones de SQL y bases de datos]]
- [ ] [[Oracle Certified Professional Java SE]]
- [ ] [[Spring Professional]]

### Arquitectura, gestión y liderazgo
- [ ] [[TOGAF]]
- [ ] [[Scrum (PSM y PSPO)]]
- [ ] [[Certificaciones de inglés (TOEFL, IELTS, Cambridge y EF SET)]]
- [ ] [[Cómo decidir qué certificación vale la pena]]
- [ ] [[Plan de certificaciones (calendario y presupuesto)]]
- [ ] [[Vouchers y descuentos de examen]]

---

## 19 · Redes (nuevo)

> De lo más básico (qué hace un switch) a lo más completo. Práctica con Cisco Packet Tracer (gratuito con cuenta de Cisco Networking Academy; Pendiente de confirmar condiciones al escribir la nota).

### Fundamentos
- [ ] [[Qué es una red (LAN, WAN, MAN, PAN)]]
- [ ] [[Topologías de red]]
- [ ] [[Modelo OSI]]
- [ ] [[Modelo TCP-IP]]
- [ ] [[Encapsulamiento y PDUs]]
- [ ] [[Medios físicos (cobre, fibra e inalámbrico)]]
- [ ] [[Cableado estructurado y estándares (T568A y T568B)]]
- [ ] [[Dispositivos de red (hub, switch, router, access point, firewall)]]
- [ ] [[Hub vs switch vs router]]
- [ ] [[Ethernet y tramas]]
- [ ] [[Direcciones MAC]]
- [ ] [[Dominios de colisión y de broadcast]]

### Switching
- [ ] [[Cómo aprende un switch (tabla MAC)]]
- [ ] [[Unicast, broadcast y multicast]]
- [ ] [[VLANs]]
- [ ] [[Trunking 802.1Q]]
- [ ] [[Routing entre VLANs (router on a stick y switch capa 3)]]
- [ ] [[STP (Spanning Tree) y RSTP]]
- [ ] [[EtherChannel (LACP y PAgP)]]
- [ ] [[Port security]]
- [ ] [[DHCP snooping y Dynamic ARP Inspection]]

### Direccionamiento IP
- [ ] [[IPv4 - estructura y clases]]
- [ ] [[Direcciones públicas y privadas]]
- [ ] [[Máscara de subred y CIDR]]
- [ ] [[Subnetting paso a paso]]
- [ ] [[VLSM]]
- [ ] [[IPv6 - direccionamiento]]
- [ ] [[IPv6 - SLAAC y DHCPv6]]
- [ ] [[ARP]]
- [ ] [[ICMP, ping y traceroute]]

### Servicios de red
- [ ] [[DHCP]]
- [ ] [[DNS en la red (resolución, registros y servidores)]] (complementa [[DNS]])
- [ ] [[NAT y PAT]]
- [ ] [[NTP]]
- [ ] [[SNMP y Syslog]]
- [ ] [[FTP, SFTP y TFTP]] 🛠 (publicación con FileZilla)
- [ ] [[QoS (calidad de servicio)]]

### Routing
- [ ] [[Cómo decide un router (tabla de rutas)]]
- [ ] [[Rutas estáticas y ruta por defecto]]
- [ ] [[Enrutamiento dinámico (conceptos)]]
- [ ] [[OSPF]]
- [ ] [[EIGRP]]
- [ ] [[BGP (conceptos)]]
- [ ] [[Redundancia de gateway (HSRP y VRRP)]]

### Inalámbrico
- [ ] [[Wi-Fi - estándares y bandas]]
- [ ] [[WLAN - controladores y access points]]
- [ ] [[Seguridad Wi-Fi (WPA2, WPA3 y 802.1X)]]

### Seguridad de redes
- [ ] [[ACLs (estándar y extendidas)]]
- [ ] [[Firewalls (stateless, stateful y de nueva generación)]]
- [ ] [[VPN (sitio a sitio y acceso remoto, IPsec y SSL)]]
- [ ] [[Segmentación y DMZ]]
- [ ] [[Zero Trust en redes]]
- [ ] [[Ataques comunes de red (spoofing, MITM, DoS)]]

### Cisco IOS y práctica
- [ ] [[Cisco Packet Tracer]]
- [ ] [[Cisco IOS - modos y comandos básicos]]
- [ ] [[Configuración inicial de un switch Cisco]]
- [ ] [[Configuración inicial de un router Cisco]]
- [ ] [[SSH y acceso seguro a dispositivos]]
- [ ] [[Respaldo de configuraciones]]
- [ ] [[Wireshark (captura y análisis de tráfico)]]
- [ ] [[Troubleshooting de redes (metodología por capas)]]
- [ ] [[Laboratorios de Packet Tracer (bitácora)]]

### Avanzado
- [ ] [[WAN y SD-WAN]]
- [ ] [[SDN (redes definidas por software)]]
- [ ] [[Automatización de redes (Python, Netmiko y Ansible)]]
- [ ] [[NETCONF y RESTCONF]]
- [ ] [[Redes en la nube (VNet, subnets, NSG y peering)]] (enlaza con 07 · Redes en Azure)
- [ ] [[Redes de contenedores (Docker y Kubernetes)]]

---

## 20 · Angular (nuevo, desde cero)

### Básico
- [ ] [[Qué es Angular y en qué se diferencia de React]]
- [ ] [[Angular CLI y estructura de un proyecto]]
- [ ] [[TypeScript para Angular]] (enlaza [[TypeScript]])
- [ ] [[Componentes en Angular]]
- [ ] [[Componentes standalone]]
- [ ] [[Templates y data binding (interpolación, propiedades y eventos)]]
- [ ] [[Two-way binding]]
- [ ] [[Control flow (@if, @for y @switch)]]
- [ ] [[Input y Output]]
- [ ] [[Ciclo de vida de componentes]]
- [ ] [[Directivas]]
- [ ] [[Pipes]]

### Intermedio
- [ ] [[Servicios e inyección de dependencias en Angular]]
- [ ] [[Signals]]
- [ ] [[computed y effect]]
- [ ] [[RxJS - Observables]]
- [ ] [[RxJS - operadores más usados]]
- [ ] [[Signals vs RxJS]]
- [ ] [[HttpClient en Angular]]
- [ ] [[Interceptores HTTP]]
- [ ] [[Router de Angular]]
- [ ] [[Guards y resolvers]]
- [ ] [[Lazy loading de rutas]]
- [ ] [[Formularios template-driven]]
- [ ] [[Formularios reactivos]]
- [ ] [[Validaciones personalizadas]]
- [ ] [[Manejo de errores en Angular]]

### Avanzado
- [ ] [[Detección de cambios y zoneless]]
- [ ] [[OnPush y rendimiento]]
- [ ] [[Estado global (NgRx y Signal Store)]]
- [ ] [[SSR e hidratación en Angular]]
- [ ] [[Deferrable views (@defer)]]
- [ ] [[Angular Material y CDK]]
- [ ] [[Internacionalización en Angular]]
- [ ] [[Pruebas en Angular (unitarias y de componentes)]]
- [ ] [[Estructura de carpetas por feature en Angular]]
- [ ] [[Angular con un API .NET (autenticación y CORS)]]
- [ ] [[Proyecto práctico en Angular]]

---

## 21 · IA y Machine Learning (nuevo)

### Matemáticas necesarias
- [ ] [[Álgebra lineal para ML (vectores, matrices y producto punto)]]
- [ ] [[Cálculo para ML (derivadas y gradiente)]]
- [ ] [[Probabilidad para ML]]
- [ ] [[Estadística para ML (distribuciones, media, varianza)]]

### Python para datos
- [ ] [[Python - básico]]
- [ ] [[Python - entornos virtuales y pip]]
- [ ] [[Jupyter notebooks]]
- [ ] [[NumPy]]
- [ ] [[pandas]]
- [ ] [[Visualización con matplotlib y seaborn]]

### Machine learning clásico
- [ ] [[Qué es el machine learning]]
- [ ] [[Aprendizaje supervisado, no supervisado y por refuerzo]]
- [ ] [[Datos de entrenamiento, validación y prueba]]
- [ ] [[Regresión lineal]]
- [ ] [[Regresión logística]]
- [ ] [[Árboles de decisión y random forest]]
- [ ] [[Gradient boosting (XGBoost y LightGBM)]]
- [ ] [[k-NN y SVM]]
- [ ] [[Clustering (k-means)]]
- [ ] [[Reducción de dimensionalidad (PCA)]]
- [ ] [[Overfitting, underfitting y sesgo-varianza]]
- [ ] [[Validación cruzada]]
- [ ] [[Métricas de clasificación y regresión]]
- [ ] [[Feature engineering]]
- [ ] [[scikit-learn]]

### Redes neuronales y deep learning
- [ ] [[Neurona artificial y perceptrón]]
- [ ] [[Funciones de activación]]
- [ ] [[Redes neuronales multicapa]]
- [ ] [[Función de pérdida]]
- [ ] [[Descenso de gradiente y backpropagation]]
- [ ] [[Optimizadores (SGD y Adam)]]
- [ ] [[Regularización y dropout]]
- [ ] [[Redes convolucionales (CNN)]]
- [ ] [[Redes recurrentes (RNN y LSTM)]]
- [ ] [[Embeddings]]
- [ ] [[Mecanismo de atención]]
- [ ] [[Transformers]]
- [ ] [[Transfer learning y fine-tuning]]
- [ ] [[PyTorch]]
- [ ] [[GPUs y por qué importan]]

### Modelos de lenguaje (LLMs)
- [ ] [[Cómo funciona un LLM (tokens, contexto y predicción)]]
- [ ] [[Parámetros de generación (temperatura y top-p)]]
- [ ] [[Prompting efectivo]] ⭐
- [ ] [[Salidas estructuradas (JSON)]]
- [ ] [[Tool calling (llamadas a herramientas)]] ⭐
- [ ] [[RAG (generación aumentada con recuperación)]] ⭐
- [ ] [[Embeddings y bases vectoriales (pgvector y Azure AI Search)]]
- [ ] [[Chunking y reranking]]
- [ ] [[Fine-tuning vs RAG vs prompting]]
- [ ] [[LoRA y ajuste eficiente]]
- [ ] [[Alucinaciones y cómo reducirlas]]
- [ ] [[Evaluación de respuestas de LLMs]] ⭐
- [ ] [[Costo y latencia de LLMs]]
- [ ] [[Modelos abiertos vs comerciales]]

### Agentes de IA
- [ ] [[Qué es un agente de IA]] ⭐
- [ ] [[El ciclo de un agente (razonar, actuar, observar)]]
- [ ] [[Herramientas y MCP (Model Context Protocol)]] ⭐🛠 (Obsidian MCP)
- [ ] [[Memoria de agentes (corto y largo plazo)]]
- [ ] [[Planificación y descomposición de tareas]]
- [ ] [[Sistemas multi-agente y orquestación]]
- [ ] [[Humano en el ciclo (aprobaciones)]]
- [ ] [[Guardrails y límites de un agente]]
- [ ] [[Evaluación de agentes]]
- [ ] [[Frameworks de agentes (comparativa)]] (se verifica en la web al escribirla)
- [ ] [[Agentes de código (asistentes de programación)]]
- [ ] [[Computer use y agentes de navegador]]

### IA en .NET y Azure
- [ ] [[Microsoft.Extensions.AI]] ⭐
- [ ] [[Semantic Kernel y Microsoft Agent Framework]] (Pendiente de confirmar estado actual)
- [ ] [[ML.NET]]
- [ ] [[ONNX Runtime]]
- [ ] [[Azure OpenAI y Azure AI Foundry]]

### MLOps y producción
- [ ] [[MLOps - conceptos]]
- [ ] [[Versionado de datos y modelos]]
- [ ] [[MLflow]]
- [ ] [[Desplegar un modelo como API]]
- [ ] [[Monitoreo de modelos y drift]]
- [ ] [[Azure Machine Learning]]

### Responsabilidad y seguridad
- [ ] [[IA responsable (sesgo, equidad y transparencia)]]
- [ ] [[Prompt injection]] ⭐
- [ ] [[OWASP Top 10 para LLMs]] ⭐
- [ ] [[Privacidad de datos al usar IA]]

### Recursos y práctica
- [ ] [[Ruta de aprendizaje de IA (de cero a agentes)]]
- [ ] [[Cursos recomendados de ML y deep learning]]
- [ ] [[Proyectos prácticos de IA]]

---

## 22 · Integraciones (nuevo)

### Pagos
- [ ] [[Pasarelas de pago - conceptos]] 🛠
- [ ] [[Cargos, intents y estados de un pago]] 🛠
- [ ] [[Pagos en efectivo y transferencia (OXXO y SPEI)]] 🛠
- [ ] [[Marketplaces y comisiones]] 🛠
- [ ] [[Conciliación de pagos]]
- [ ] [[PCI DSS]]
- [x] [[Stripe]] _(existe)_ 🛠
- [x] [[Stripe Connect]] _(existe)_ 🛠
- [x] [[OpenPay]] _(existe)_ 🛠

### Correo
- [ ] [[Correo transaccional (SMTP y proveedores)]] 🛠
- [ ] [[SPF, DKIM y DMARC]]

### Documentos y archivos
- [ ] [[Generar documentos Word desde .NET (Open XML)]] 🛠 (Pendiente de confirmar la librería de ReporteriaAzure)
- [ ] [[Generar Excel desde .NET]]
- [x] [[ImageSharp]] _(existe)_ 🛠

### Escritorio y hardware
- [x] [[Electron]] _(existe)_ 🛠
- [x] [[Preload y contextBridge]] _(existe)_ 🛠
- [x] [[SDK BD Rowa y WWKS2]] _(existe)_ 🛠

---

## 30 · Fotografía (hobby)

> Ruta por etapas: iPhone 15 Pro (ahora) → DJI Osmo Pocket 4P → Sony a6700. Programas gratuitos.

### Fundamentos de la cámara
- [ ] [[Tipos de cámara (celular, compacta, mirrorless y réflex)]]
- [ ] [[Tamaño de sensor y factor de recorte]] ⭐
- [ ] [[Distancia focal y tipos de lente]] ⭐
- [ ] [[Modos de disparo (Auto, P, A, S y M)]] ⭐
- [ ] [[RAW vs JPEG y HEIF]] ⭐
- [ ] [[Tarjetas, baterías y cuidado del equipo fotográfico]]

### Exposición
- [ ] [[Triángulo de exposición]] ⭐
- [ ] [[Apertura y profundidad de campo]] ⭐
- [ ] [[Velocidad de obturación y movimiento]] ⭐
- [ ] [[ISO y ruido digital]]
- [ ] [[Medición de luz (matricial, ponderada y puntual)]]
- [ ] [[Compensación de exposición]]
- [ ] [[Histograma y rango dinámico]] ⭐
- [ ] [[Bracketing y HDR en fotografía]]

### Enfoque
- [ ] [[Modos de enfoque automático (AF-S, AF-C y seguimiento)]]
- [ ] [[Enfoque manual y focus peaking]]
- [ ] [[Distancia hiperfocal]]
- [ ] [[Focus stacking (apilado de enfoque)]]

### Luz y color
- [ ] [[Balance de blancos y temperatura de color]] ⭐
- [ ] [[Luz dura y luz suave]]
- [ ] [[Dirección de la luz]]
- [ ] [[Hora dorada y hora azul]]
- [ ] [[Teoría del color y armonías]] ⭐
- [ ] [[Flash (TTL, manual, rebotado y alta velocidad)]]
- [ ] [[Modificadores de luz (difusor, reflector y softbox)]]
- [ ] [[Esquemas de iluminación de retrato]]

### Composición
- [ ] [[Regla de tercios y proporción áurea]] ⭐
- [ ] [[Líneas guía, encuadre natural, simetría y patrones]]
- [ ] [[Espacio negativo y simplicidad]]
- [ ] [[Punto de vista y capas de profundidad]]
- [ ] [[Educar el ojo (analizar fotos y fotógrafos)]] ⭐

### Géneros
- [ ] [[Fotografía de retrato]]
- [ ] [[Fotografía de paisaje]]
- [ ] [[Fotografía de calle]]
- [ ] [[Fotografía de arquitectura]]
- [ ] [[Fotografía de producto y comida]]
- [ ] [[Fotografía macro]]
- [ ] [[Fotografía nocturna y astrofotografía]]
- [ ] [[Larga exposición]]
- [ ] [[Fotografía de deporte y acción]]
- [ ] [[Fotografía de eventos]]
- [ ] [[Fotografía de viaje]]
- [ ] [[Fotografía de fauna]]

### Avanzado
- [ ] [[Filtros (ND, polarizador y degradado)]]
- [ ] [[Fotografía panorámica]]
- [ ] [[Time-lapse]]
- [ ] [[Fotografía de estudio y tethering]]
- [ ] [[Fotografía computacional en celulares]] 🛠

### Flujo, publicación y legal
- [ ] [[Importar, nombrar y organizar fotos]]
- [ ] [[Respaldo 3-2-1 de fotos y videos]] ⭐
- [ ] [[Metadatos EXIF e IPTC]]
- [ ] [[Derechos de autor de fotos y videos]]
- [ ] [[Permiso de modelo y fotografía en espacios públicos en México]]
- [ ] [[Portafolio fotográfico y dónde publicar]]
- [ ] [[Pedir y dar crítica fotográfica]]

---

## 31 · Video y audio (hobby)

### Técnica de video
- [ ] [[Resolución y cuadros por segundo]] ⭐
- [ ] [[Regla de los 180 grados del obturador]] ⭐
- [ ] [[Cámara lenta (high frame rate)]]
- [ ] [[Códecs y contenedores de video (H.264, H.265 y ProRes)]]
- [ ] [[Bitrate y tamaño de archivo]]
- [ ] [[Profundidad de bits y submuestreo de croma]]
- [ ] [[Perfiles de color en video (estándar, Log y HLG)]] ⭐
- [ ] [[Herramientas de exposición en video (zebras, waveform y false color)]]
- [ ] [[Filtros ND en video]]

### Lenguaje audiovisual
- [ ] [[Tipos de plano]] ⭐
- [ ] [[Ángulos de cámara]]
- [ ] [[Movimientos de cámara]] ⭐
- [ ] [[Eje de 180 grados y continuidad]]
- [ ] [[B-roll y plano de ubicación]]
- [ ] [[Narrativa y storytelling en video]] ⭐

### Audio
- [ ] [[Tipos de micrófono (lavalier, shotgun y de cámara)]]
- [ ] [[Ganancia y niveles de audio]] ⭐
- [ ] [[Colocación del micrófono]]
- [ ] [[Sonido ambiente y voz en off]]
- [ ] [[Grabar entrevistas a dos personas]] 🛠
- [ ] [[Sincronizar audio y video]]

### Producción
- [ ] [[Iluminación de tres puntos]]
- [ ] [[Luz LED continua]]
- [ ] [[Estabilización (tripié, gimbal y estabilizador de cámara)]]
- [ ] [[Preproducción (idea, guion y guion técnico)]]
- [ ] [[Storyboard y lista de tomas]]
- [ ] [[Plan de rodaje]]

### Formatos
- [ ] [[Video vertical (Reels, TikTok y Shorts)]] ⭐
- [ ] [[Video para YouTube]]
- [ ] [[Entrevista en video]]
- [ ] [[Vlog]]
- [ ] [[Tutorial y screencast]]
- [ ] [[Video de producto]]
- [ ] [[Cortometraje]]
- [ ] [[Time-lapse e hyperlapse en video]]
- [ ] [[Creación de contenido (ganchos, retención y constancia)]]

---

## 32 · Edición de fotografía (hobby)

### Fundamentos
- [ ] [[Revelado RAW vs retoque]]
- [ ] [[Edición no destructiva]]
- [ ] [[Flujo de edición de fotos]] ⭐
- [ ] [[Espacios de color (sRGB, Adobe RGB y Display P3)]]
- [ ] [[Calibración del monitor]]

### Programas gratuitos
- [ ] [[darktable y Ansel]] ⭐
- [ ] [[RawTherapee]]
- [ ] [[GIMP]]
- [ ] [[Photopea]]
- [ ] [[Snapseed]]
- [ ] [[Lightroom móvil (versión gratuita)]]

### Revelado
- [ ] [[Exposición, blancos y negros en el revelado]] ⭐
- [ ] [[Curvas de tonos]]
- [ ] [[HSL y gradación de color]]
- [ ] [[Nitidez y reducción de ruido]]
- [ ] [[Corrección de lente y perspectiva]]
- [ ] [[Recorte y enderezado]]

### Ajustes locales y retoque
- [ ] [[Máscaras y ajustes locales]] ⭐
- [ ] [[Dodge and burn]]
- [ ] [[Clonar y parchar]]
- [ ] [[Retoque de piel con separación de frecuencias]]
- [ ] [[Ética del retoque]]

### Capas y composición
- [ ] [[Capas, máscaras y modos de fusión]]
- [ ] [[Selecciones y quitar fondos]]
- [ ] [[Fotomontaje]]

### Look y técnicas
- [ ] [[Presets y LUTs en fotografía]]
- [ ] [[Consistencia de color en una serie]]
- [ ] [[Blanco y negro]]
- [ ] [[Unir HDR y panorámicas]]

### IA en la edición
- [ ] [[Relleno generativo y quitar objetos]]
- [ ] [[Reducción de ruido con IA]]
- [ ] [[Ética y derechos de las imágenes con IA]]

### Exportación
- [ ] [[Formatos de exportación de imagen (JPEG, PNG, TIFF, HEIC, WebP y AVIF)]]
- [ ] [[Tamaños para web y redes sociales]]
- [ ] [[Marca de agua]]
- [ ] [[Impresión fotográfica (DPI, perfiles y prueba en pantalla)]]

---

## 33 · Edición de video (hobby)

### Fundamentos
- [ ] [[DaVinci Resolve (versión gratuita)]] ⭐
- [ ] [[Kdenlive y Shotcut]]
- [ ] [[CapCut]]
- [ ] [[Proyecto, bins y línea de tiempo]]
- [ ] [[Proxies y rendimiento al editar]]
- [ ] [[Organizar material de video]]

### Montaje
- [ ] [[Tipos de corte (J-cut, L-cut, jump cut y match cut)]] ⭐
- [ ] [[Ritmo y narrativa en el montaje]] ⭐
- [ ] [[Editar al ritmo de la música]]
- [ ] [[Uso del b-roll en la edición]]

### Audio en posproducción
- [ ] [[Limpieza de ruido en audio]]
- [ ] [[Ecualización y compresión de voz]]
- [ ] [[Loudness (LUFS) por plataforma]]
- [ ] [[Mezcla de voz y música]]
- [ ] [[Audacity]]
- [ ] [[Música y efectos libres de regalías]]

### Color
- [ ] [[Corrección de color vs gradación]] ⭐
- [ ] [[Scopes (waveform, vectorscopio y parade)]] ⭐
- [ ] [[Material Log y LUTs]]
- [ ] [[Nodos de color en DaVinci Resolve]]
- [ ] [[Igualar tomas y tonos de piel]]

### Gráficos y efectos
- [ ] [[Títulos y lower thirds]]
- [ ] [[Subtítulos (SRT)]]
- [ ] [[Keyframes y animación básica]]
- [ ] [[Transiciones con criterio]]
- [ ] [[Chroma key]]
- [ ] [[Máscaras y tracking en video]]
- [ ] [[Estabilización en posproducción (Gyroflow)]]
- [ ] [[Rampas de velocidad]]
- [ ] [[Motion graphics básico (Fusion)]]

### Entrega
- [ ] [[Exportar video (códec, bitrate y formato por plataforma)]]
- [ ] [[Miniaturas y diseño básico]]
- [ ] [[HandBrake]]
- [ ] [[OBS Studio]]
- [ ] [[Content ID y derechos en plataformas]]

---

## 34 · Equipo (hobby)

### Mi equipo actual
- [ ] [[iPhone 15 Pro para foto y video]] 🛠
- [ ] [[Blackmagic Camera (app)]] 🛠
- [ ] [[DJI Mic Mini 2]] 🛠
- [ ] [[DJI Mini 4K]] 🛠

### Próximo equipo
- [ ] [[DJI Osmo Pocket 4P]]
- [ ] [[Sony a6700]]
- [ ] [[Lentes para la Sony a6700]]
- [ ] [[Sony S-Cinetone y S-Log3]]

### Compras y accesorios
- [ ] [[Qué comprar primero (guía para principiantes)]] ⭐
- [ ] [[Tarjetas de memoria (clases de velocidad)]]
- [ ] [[SSD externo para grabar y respaldar]]
- [ ] [[Tripiés y soportes]]
- [ ] [[Filtros ND y Black Mist para video]]
- [ ] [[Luces LED portátiles]]
- [ ] [[Computadora para editar foto y video]]
- [ ] [[Cuidado del equipo en viaje, calle y lluvia]]

### Certificaciones opcionales
- [ ] [[Certificación de DaVinci Resolve (Blackmagic)]]
- [ ] [[Adobe Certified Professional (Photoshop y Premiere)]]

---

## 35 · Drones (hobby)

### Normativa y seguridad
- [ ] [[NOM-107-SCT3-2019 y registro ante la AFAC]] ⭐
- [ ] [[Categorías de uso de drones (recreativo, privado y comercial)]]
- [ ] [[Zonas restringidas y privacidad al volar]]
- [ ] [[Revisión antes de despegar (checklist)]] ⭐
- [ ] [[Viento, clima y retorno automático (RTH)]]
- [ ] [[Baterías de dron (cuidado y viajes)]]

### Pilotaje
- [ ] [[Controles y modos de vuelo (Cine, Normal y Sport)]]
- [ ] [[GPS, calibración y sensores del dron]]
- [ ] [[Simuladores de vuelo de drones]]
- [ ] [[Maniobras básicas con dron]] ⭐
- [ ] [[QuickShots y modos automáticos]]

### Foto y video aéreo
- [ ] [[Composición aérea y cenital]]
- [ ] [[Panorámicas y HDR con dron]]
- [ ] [[Hyperlapse con dron]]
- [ ] [[Exposición en video aéreo (ND y 180 grados)]]
- [ ] [[Movimientos cinematográficos con dron]]

### Edición y mantenimiento
- [ ] [[Edición de material aéreo]]
- [ ] [[Mantenimiento del dron (firmware y hélices)]]

---

## 36 · Impresión 3D (hobby)

> Impresora: Elegoo Centauri 2 Combo (marco abierto, CoreXY, multicolor CANVAS). Programas gratuitos.

### Fundamentos
- [ ] [[Qué es la impresión 3D FDM (y diferencias con resina y SLS)]] ⭐
- [ ] [[Partes de una impresora FDM (CoreXY, extrusor, hotend y cama)]]
- [ ] [[Elegoo Centauri 2 Combo]] 🛠
- [ ] [[Sistema multicolor CANVAS de Elegoo]] 🛠
- [ ] [[Seguridad al imprimir (ventilación, emisiones y ubicación)]] ⭐

### Laminador
- [ ] [[ElegooSlicer y OrcaSlicer]] ⭐
- [ ] [[Altura de capa y ancho de línea]]
- [ ] [[Perímetros y relleno]] ⭐
- [ ] [[Soportes (normales, árbol e interfaces)]] ⭐
- [ ] [[Adherencia a la cama (brim, raft y superficies)]]
- [ ] [[Velocidades, aceleraciones y enfriamiento]]
- [ ] [[Costura, ironing y altura de capa variable]]
- [ ] [[Impresión multicolor (pintar colores y torre de purga)]] 🛠
- [ ] [[Perfiles de filamento e impresora]]
- [ ] [[Archivos para imprimir (STL, 3MF, OBJ y STEP)]]

### Materiales
- [ ] [[PLA]] ⭐
- [ ] [[PETG]]
- [ ] [[TPU (filamento flexible)]]
- [ ] [[Filamentos especiales (seda, mate, madera y brillo)]]
- [ ] [[Materiales que necesitan impresora cerrada (ABS, ASA, PC y nylon)]]
- [ ] [[Secado y almacenamiento de filamento]] ⭐

### Calibración y calidad
- [ ] [[Primera capa y calibración automática]] ⭐
- [ ] [[Calibración de flujo y pressure advance]]
- [ ] [[Torre de temperatura]]
- [ ] [[Calibración de retracción]]
- [ ] [[Prueba de tolerancias]]
- [ ] [[Problemas comunes de impresión FDM]] ⭐
- [ ] [[Mantenimiento de la impresora 3D]]

### Posprocesado
- [ ] [[Quitar soportes y limpiar piezas]]
- [ ] [[Lijado y relleno de piezas impresas]]
- [ ] [[Primer, pintura y barniz para piezas impresas]]
- [ ] [[Insertos térmicos, tornillería y pegado]]

### Herramientas y compras
- [ ] [[Herramientas básicas para impresión 3D]] ⭐
- [ ] [[Calibrador digital (vernier)]]
- [ ] [[Secador de filamento]]
- [ ] [[Tablet para escultura digital (opcional)]]

### Comunidad, licencias y costos
- [ ] [[Repositorios de modelos (Printables, MakerWorld y Thingiverse)]]
- [ ] [[Licencias de modelos 3D (Creative Commons y uso comercial)]]
- [ ] [[Personajes y marcas registradas en impresión 3D]]
- [ ] [[Costeo de una impresión 3D]]

---

## 37 · Modelado 3D técnico (hobby)

### Programas gratuitos
- [ ] [[Tinkercad]] ⭐
- [ ] [[Onshape (plan gratuito)]] ⭐
- [ ] [[FreeCAD]]
- [ ] [[Fusion (licencia personal)]]
- [ ] [[OpenSCAD]]

### Modelado paramétrico
- [ ] [[Bocetos, restricciones y cotas]] ⭐
- [ ] [[Extruir, revolucionar, barrer y recubrir]] ⭐
- [ ] [[Chaflanes, redondeos, vaciado y patrones]]
- [ ] [[Parámetros y diseño por variables]]
- [ ] [[Ensambles y uniones en CAD]]

### Diseño para impresión 3D
- [ ] [[Orientación de la pieza y resistencia por capas]] ⭐
- [ ] [[Voladizos y puentes]]
- [ ] [[Tolerancias y holguras para piezas que encajan]] ⭐
- [ ] [[Agujeros, roscas impresas, insertos y tuercas atrapadas]]
- [ ] [[Clips, bisagras impresas y piezas en varias partes]]
- [ ] [[Grosor de pared y relleno según la carga]]
- [ ] [[Medir piezas e ingeniería inversa]]

### Proyectos técnicos
- [ ] [[Soportes y organizadores impresos]]
- [ ] [[Repuestos y adaptadores impresos]]
- [ ] [[Cajas para electrónica]]
- [ ] [[Accesorios impresos para foto, video y dron]] 🛠

---

## 38 · Modelado 3D artístico (hobby)

### Blender
- [ ] [[Blender - interfaz, navegación y modos]] ⭐
- [ ] [[Modelado poligonal en Blender]] ⭐
- [ ] [[Modificadores en Blender]]
- [ ] [[Escultura digital en Blender]] ⭐
- [ ] [[Retopología básica]]
- [ ] [[Preparar un modelo de Blender para imprimir]] ⭐

### Otras herramientas
- [ ] [[Escultura en tablet (Nomad Sculpt y alternativas)]]
- [ ] [[Reparar mallas para impresión]]

### Técnicas artísticas
- [ ] [[Proporciones y anatomía básica para figuras]]
- [ ] [[Cortar modelos en piezas y pines de unión]]
- [ ] [[Bustos y figuras originales]]
- [ ] [[Dioramas]]
- [ ] [[Litofanías]]
- [ ] [[Macetas, lámparas y objetos decorativos]]
- [ ] [[Modelos pensados para impresión multicolor]] 🛠
- [ ] [[Pintura de figuras impresas]]

### Escaneo 3D
- [ ] [[Fotogrametría y escaneo con iPhone]] 🛠
- [ ] [[Limpiar un escaneo y hacerlo imprimible]]

---

## 39 · IA para modelado 3D (hobby)

> PC con GTX 1660 Ti (6 GB de VRAM): modelos ligeros en local; lo pesado, en la nube gratuita. Enlaza con 21 · IA y 38 · Modelado artístico.

### Conceptos
- [ ] [[Texto a 3D e imagen a 3D]] ⭐
- [ ] [[Representaciones 3D (malla, NeRF, Gaussian splatting y campos implícitos)]]
- [ ] [[Cómo funciona un modelo de imagen a 3D]]
- [ ] [[Servicios comerciales de IA 3D (Tripo y similares)]]

### Entorno local
- [ ] [[Python, CUDA y PyTorch para IA 3D en Windows]] ⭐
- [ ] [[VRAM - medirla y qué hacer cuando no alcanza]] ⭐
- [ ] [[Tarjetas GTX 16 y media precisión (FP16)]]
- [ ] [[ComfyUI para generación 3D]]
- [ ] [[Interfaces Gradio de los modelos 3D]]

### Modelos
- [ ] [[TripoSR]] ⭐
- [ ] [[Hunyuan3D-2mini]] ⭐
- [ ] [[Stable Fast 3D]]
- [ ] [[TRELLIS]]
- [ ] [[Modelos de imagen a 3D en la nube gratuita (Hugging Face y Colab)]]

### Flujo completo
- [ ] [[Crear la imagen de entrada para IA 3D]] ⭐
- [ ] [[Stable Diffusion en 6 GB de VRAM]]
- [ ] [[Quitar el fondo de una imagen]]
- [ ] [[Limpiar un modelo generado por IA en Blender]] ⭐
- [ ] [[Separar un modelo por colores para CANVAS]] 🛠
- [ ] [[De la IA a la impresora (flujo completo)]] ⭐

### Calidad y criterio
- [ ] [[Qué imágenes funcionan mejor para IA 3D]]
- [ ] [[Errores típicos de los modelos generados por IA]]
- [ ] [[IA vs CAD vs escaneo (cuándo usar cada uno)]]

### Licencias y ética
- [ ] [[Licencias de modelos de IA 3D]]
- [ ] [[Propiedad y uso comercial de modelos generados por IA]]

---

## Recursos transversales

- [ ] [[Glosario]]
- [ ] [[Errores y soluciones]]
- [ ] [[Checklist de despliegue]]
- [ ] [[Checklist de revisión de código]]
- [ ] [[Checklist de nuevo proyecto]]
- [ ] [[Snippets reutilizables]]
- [ ] [[Plantilla de nota]] (ver frontmatter al inicio)

---

## Pendiente

- [x] Sección de hobbies: fotografía, video y audio, edición, equipo y drones (áreas 30–35).
- [ ] Tabla "qué va a dónde" para mover las notas de `Conocimiento/` y las carpetas de proyectos a la estructura numerada.
