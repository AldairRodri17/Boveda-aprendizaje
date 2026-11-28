---
proyecto: [todos]
tipo: estructura
estado: vigente
tags: [estructura, reglas, claude, inicio-proyecto, arquitectura, nombres]
actualizado: 2026-11-09
contenido: completo
---
# Reglas para crear proyectos

Índice: [[Estructura proyectos]] · Por capa: [[Reglas Back - .NET]] · [[Reglas Front - SPA]]

> [!important] Resumen para Claude
> 1. **Si tienes una duda, no inventes: pregunta.**
> 2. Nombres: proyecto = dios mesoamericano, sistema = dios griego (`Proyecto.Sistema`), base de datos = `db` + dios egipcio + instancia.
> 3. **Antes de escribir código se evalúa la arquitectura** y se documenta en `ARQUITECTURA.md` en la raíz del repo (plantilla en la sección 11).
> 4. Nada de capas, interfaces ni patrones por costumbre: cada uno se justifica en `ARQUITECTURA.md`.
> 5. Se borra todo lo que no se usa.

> [!info] Choques con reglas anteriores
> Todos resueltos en [[#Choques con reglas anteriores]] (al final).

---

## 1. Nombres

1. El proyecto lleva nombre de dios mesoamericano y cada sistema o servicio, de dios griego (`Proyecto.Sistema`).
2. Los nombres deben ser fáciles de pronunciar.
3. Cada sistema tiene identidad propia.
4. El namespace usa el nombre del proyecto en toda la solución.
5. Las bases de datos se nombran `db` + dios egipcio + instancia.
6. Backend y frontend van en repos separados, con el mismo nombre de proyecto.

Ejemplos en la bóveda: [[Ocelotl.Hermes]] / [[Ocelotl.Iris]], [[Itzamna.Dionisio]] / [[Itzamna.Clio]], [[Pochteca.Nomisma]].

---

## 2. Arquitectura: se evalúa y se documenta

1. **Antes de escribir código, cada proyecto (o servicio nuevo) evalúa qué arquitectura necesita.** Se consideran [[Vertical Slice Architecture|Vertical Slice]], [[Arquitectura hexagonal|Hexagonal]] (puertos y adaptadores), [[Clean Architecture]], monolito modular u otra, y se elige la que encaje con el problema.
2. **La evaluación mira estos criterios:**
   - cuánta lógica de dominio hay, frente a un CRUD con SQL;
   - cuántos sistemas externos hay (SDK, pasarelas, PAC, robots) y qué tanto cambian;
   - si hará falta probar sin esos sistemas externos;
   - si habrá varios dominios que puedan separarse más adelante;
   - el tamaño del equipo y cuánto va a crecer el proyecto;
   - si es un sistema nuevo o convive con uno legacy.
3. **Se documenta la decisión** en un archivo `ARQUITECTURA.md` en la raíz del repo (ver plantilla en la sección 11).
4. **Si el proyecto cambia** (nuevo sistema externo, crecimiento, cambio de alcance), se revisa la decisión y se actualiza el documento, sin borrar el historial.
5. **No se agregan capas, interfaces ni patrones por costumbre.** Cada uno debe poder justificarse con un criterio de la regla 2.
6. **Se pueden combinar enfoques** (por ejemplo, organizar por slices y usar puertos solo en los bordes externos), pero la combinación también se documenta.
7. **Un slice o módulo no llama a los internos de otro**; solo usa su contrato público. Esto vale para cualquier arquitectura.
8. **Se borra todo lo que no se usa:** métodos, endpoints, variables y archivos.

---

## 3. Backend (.NET)

1. Los proyectos nuevos usan la versión más reciente de .NET.
2. La estructura de carpetas sigue lo que diga `ARQUITECTURA.md`. Si la decisión es Vertical Slice, cada slice contiene su request, handler, validación y acceso a datos.
3. Acceso a datos con [[Dapper]], sin [[Entity Framework Core|Entity Framework]]. [[SQL Server]] con stored procedures; [[PostgreSQL|Postgres]] con Npgsql. **En cualquier base, no hay SQL escrito en C#:** cada llamada (consulta, SP o función) es un archivo `.sql` con su propia interfaz ([[SQL en archivos con una interfaz por consulta]], [[Reglas Back - .NET]] §4).
4. [[FluentValidation]] como pipeline behavior. **El mapeo se hace a mano** (sin AutoMapper ni librerías de mapeo).
5. Los controllers o endpoints son delgados y no llevan lógica de negocio.
6. Los secretos se validan al arrancar. **Dónde van lo decide la forma de publicar:** si el servidor tiene variables de entorno o un almacén de secretos, ahí (user-secrets en desarrollo); si se publica copiando la carpeta (FileZilla), en `appsettings` con un token de alcance mínimo. Preguntar antes de moverlos ([[Secretos con User Secrets]], [[ReporteriaAzure]]).
7. Los eventos se resuelven inyectando la interfaz directo en el observer, sin MediatR Notifications.
8. `DependencyInjection.cs` es estático. Los observers se registran antes que los adapters, y los que mantienen conexiones persistentes van como Singleton.
9. La API incluye Swagger con `CustomSchemaIds` y un Dockerfile.
10. Los scripts SQL van versionados en el proyecto.

---

## 4. Frontend en JS plano (módulos con capas)

Hay **dos variantes**; `ARQUITECTURA.md` dice cuál usa el proyecto:
- **Sin bundler** (valor por defecto): esta sección, con tres archivos por módulo (`Service` · `Handler` · `Events`).
- **Con [[Vite]] y módulos ES**: [[Reglas Front - SPA]] (molde de [[Ocelotl.Iris]], seis archivos con `view` y contrato `Acciones`). Comparten el estilo de código de abajo.

1. JavaScript ES2022 sin TypeScript ni bundler. Cada archivo se carga con `<script>` y expone su clase en `window`. Todo el texto visible está en español.
2. Cada módulo es una carpeta con `<Modulo>.html`, `<Modulo>.css` (si tiene estilos) y `JS/` con tres archivos: `.Service.js`, `.Handler.js` y `.Events.js`. Los scripts se cargan siempre en ese orden.
3. Los modales son módulos normales, con su propio HTML y sus tres JS.
4. **Service:** endpoints y llamadas a `Api`, mapea los nombres al dominio y normaliza la respuesta. No toca el DOM, no notifica y no lee el estado de la pantalla. En los guardados convierte `ApiError` en `true/false/null`.
5. **Handler:** lógica, render, validaciones, notificaciones y modales. No registra eventos.
6. **Events:** instancia el Handler, llama `init()` y conecta cada evento a un método del Handler. No lleva lógica. Para elementos generados por un render usa delegación desde su contenedor.
7. Cada módulo es autocontenido. No llama al Service de otro módulo, solo a su contrato público (`fnValidar<Seccion>`, `fnValores`, `fnAbrir<X>`).
8. Toda sección expone `fnValidar<Seccion>()`: funciona sin `init()`, devuelve `true` si no aplica, marca su pestaña y no notifica.
9. Ningún texto, selector, ruta, límite o número mágico va suelto dentro de un método. Todo va en bloques `static #` congelados con `Object.freeze` (`#Selectores`, `#Mensajes`, `#Limites`…).
10. Sin `try/catch` del API en el Handler. Se evitan los dobles envíos con un booleano `#isGuardando`.
11. Las listas de HTML se arman con `forEach`, nunca con `map(...).join('')`. Los datos del API se escapan antes de insertarlos.
12. Nunca `alert`, `confirm`, `onclick` inline, estilos inline, scripts inline ni `fetch` directo (siempre `Api`).
13. No se usan globales legacy. Si algo no está en el estado compartido, se pregunta antes de leerlo de otro lado.

### Estilo del código JS
1. Métodos con prefijo `fn` + PascalCase (excepto `init`, `constructor` y los `static get`).
2. Variables y parámetros en PascalCase. Los booleanos llevan prefijo `is`. Las variables con HTML llevan `html`.
3. Instancia en Events: `_<Modulo>Handler`. Alias: `S` para selectores, `M` para mensajes, `P` para parámetros y `L` para límites.
4. 4 espacios, sin punto y coma, comillas simples. Fin de línea CRLF, y BOM UTF-8 en `.html` y `.css`.
5. Nunca dos líneas en blanco seguidas. La clase empieza en la columna 0.
6. Los comentarios son solo JSDoc de una línea, en español, y explican un contrato o un porqué.

Origen del formato: [[Guía GMM (original)]] · Molde relacionado: [[Molde Service-View-Handlers-Events]].

---

## 5. Frontend en React

1. [[React]] + [[Vite]] + [[React Router]] v6, con [[TypeScript]] donde el proyecto lo pida.
2. Una carpeta por feature (`api`, `components`, `hooks`, `pages`), sujeta a la evaluación de la sección 2.
3. Se aplica la misma regla de módulo autocontenido: una feature no usa los internos de otra.

---

## 6. Migrar un módulo legacy

1. Bajar toda la lógica del legacy: carga, eventos, validaciones, endpoints y mensajes.
2. Revisar el back de cada endpoint: qué valida, qué tablas devuelve y cuándo falla.
3. Listar código muerto y errores del legacy, y preguntar antes de omitirlos o corregirlos.
4. Preguntar todo lo que no se pueda deducir.
5. Probar y reportar qué se probó y qué falta verificar contra el API real.
6. Al terminar, borrar el legacy que ya no se usa.

---

## 7. Infraestructura y repos

1. Azure y Azure DevOps ([[Azure Pipelines]]).
2. Ramas QA y Prod, y Dev solo si hace falta.
3. En proyectos personales, solo tiers gratuitos y sin crédito promocional.
4. El dominio propio se deja para el final.

---

## 8. Autenticación

1. La autenticación se delega al servicio central de identidad ([[Hecate]]).
2. Mientras no esté listo, se usan API keys o un login temporal.
3. La integración con el servicio de identidad se hace al final.

---

## 9. Calidad y observabilidad

1. Sentry para errores, con DSN en configuración, entornos separados y datos sensibles filtrados desde el inicio.
2. La bitácora de auditoría de negocio es independiente de Sentry ([[Borrado lógico y auditoría]]).
3. La documentación es técnica y detallada, con ejemplos de código.

---

## 10. Prohibido

1. Inventar endpoints, campos, reglas o textos que no estén confirmados.
2. Agregar capas, interfaces o patrones sin justificarlos en `ARQUITECTURA.md`.
3. Lógica en Events, eventos en el Handler, DOM o notificaciones en el Service.
4. Llamar al Service de otro módulo o manipular su DOM.
5. Texto, selectores, rutas o números mágicos sueltos en el código.
6. Secretos en el repositorio o sin validar al arrancar.
7. Código muerto o comentado.

---

## 11. Plantilla de `ARQUITECTURA.md`

```md
# Arquitectura de <Proyecto.Sistema>

## Contexto
Qué hace el sistema, qué sistemas externos usa, qué tan grande es y qué se espera que crezca.

## Opciones consideradas
- Vertical Slice: ...
- Hexagonal: ...
- Otra: ...

## Decisión
La arquitectura elegida (o la combinación) y la estructura de carpetas resultante.

## Por qué
Los criterios de la sección 2 que pesaron en la decisión.

## Consecuencias
- Lo que se gana:
- Lo que se paga:

## Historial de cambios
- AAAA-MM-DD: decisión inicial.
```

---

## 12. Checklist antes de entregar

- [ ] Existe `ARQUITECTURA.md` y refleja la estructura real del proyecto.
- [ ] Los nombres siguen la sección 1.
- [ ] No hay capas ni patrones sin justificar.
- [ ] Ningún módulo usa los internos de otro.
- [ ] No hay secretos en el repositorio y los obligatorios se validan al arrancar.
- [ ] No queda código muerto ni legacy sin uso.
- [ ] En JS plano: Service / Handler / Events respetan sus responsabilidades y el estilo de la sección 4.
- [ ] Se reportó qué se probó y qué falta verificar.

---

## Choques con reglas anteriores

Al agregar estas reglas (2026-11-09) chocaban con decisiones anteriores. Así quedaron:

| # | Tema | Decisión |
|---|---|---|
| 1 | SQL en Postgres | **Archivos `.sql` siempre**, en cualquier base (§3.3 corregida) |
| 2 | AutoMapper | **Mapeo a mano** (§3.4 corregida) |
| 3 | Secretos | **Dónde van lo decide la forma de publicar**; siempre se validan al arrancar (§3.6 corregida) |
| 4 | Front JS | **Dos variantes**: sin bundler (§4, tres archivos) o con Vite ([[Reglas Front - SPA]], seis archivos); `ARQUITECTURA.md` dice cuál |
| 5 | Entity Framework | **Sin EF también en los proyectos existentes**: [[Itzamna.Dionisio]] se migra a Dapper + stored procedures (2026-11-09) |
| 6 | `ARQUITECTURA.md` | **En todos los repos**, también los existentes (creado el 2026-11-09 en Hermes, Iris, Dionisio, Clio, ReporteriaAzure y Nomisma). La nota `<Proyecto> - Arquitectura` de la bóveda lo explica con más detalle |
