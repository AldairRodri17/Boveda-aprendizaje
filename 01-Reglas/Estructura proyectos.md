---
proyecto: [Ocelotl.Iris, Ocelotl.Hermes]
tipo: estructura
estado: vigente
tags: [estructura, reglas, claude, inicio-proyecto]
actualizado: 2026-11-08
---
# Estructura proyectos

> [!important] Para Claude: lee esto antes de empezar un proyecto
> Esta carpeta guarda las reglas con las que se construyen los proyectos. Antes de escribir código en un proyecto nuevo (o en uno existente que no tenga reglas propias):
> 1. Lee esta nota, [[Reglas para crear proyectos]] (nombres, evaluación de arquitectura y `ARQUITECTURA.md`) y la de la capa que vas a tocar: [[Reglas Back - .NET]] y/o [[Reglas Front - SPA]].
> 2. Si el proyecto es el front web de GMM (scripts globales, sin módulos ES), la referencia es [[Guía GMM (original)]].
> 3. Revisa los **Aprendizajes** de los proyectos parecidos: [[Ocelotl.Hermes - Aprendizajes]] y [[Ocelotl.Iris - Aprendizajes]].
> 4. Si una regla no cubre el caso, imita lo que ya existe en el proyecto. Si aun así hay duda, **pregunta; no inventes**.

## Regla de oro
**Si tienes una duda, no inventes: pregunta.** Aplica a endpoints, nombres de campos, reglas de negocio, textos y comportamiento del sistema anterior. Lo que no se pueda confirmar en el código se marca como *Pendiente de confirmar*.

## Las reglas
| Nota | Para | Resumen |
|---|---|---|
| [[Reglas para crear proyectos]] | Todo proyecto nuevo | Nombres (dioses mesoamericanos / griegos / egipcios) · **evaluar y documentar la arquitectura en `ARQUITECTURA.md`** · .NET, JS plano, React, legacy, infra, auth, Sentry · checklist |
| [[Reglas Back - .NET]] | APIs .NET | **Controllers** (no Minimal APIs) · vertical slices · **sin SQL en C#**: un `.sql` y una interfaz por consulta · ProblemDetails con `codigo` |
| [[Reglas Front - SPA]] | Fronts en JS (Electron / web) con módulos ES | **Vite** · molde html/css/service/view/handlers/events · contrato `Acciones` · componentes `fnUi*` · formato GMM |
| [[Guía GMM (original)]] | Front web de GMM (legacy) | La guía de estilo original, tal cual; base del formato de las demás |

## Decisiones (historial)
| Fecha | Decisión | Por qué | Dónde se aplicó |
|---|---|---|---|
| 2026-11-08 | Las APIs usan **Controllers** con `[ApiController]`, no Minimal APIs | Estructura explícita por funcionalidad, filtros por atributo y convención conocida por el equipo | [[Ocelotl.Hermes]] |
| 2026-11-08 | **No hay SQL escrito en C#.** Cada consulta es un `.sql` incrustado con **su propia interfaz** (campos → respuesta) | El SQL se lee, revisa y versiona aparte; cada consulta tiene un contrato tipado. Se eligió sobre "una interfaz genérica" y sobre EF Core | [[Ocelotl.Hermes]] |
| 2026-11-08 | ~~Con **EF Core** (LINQ) se cumple la regla "sin SQL en C#"; un proyecto con EF Core no se migra a Dapper solo por la regla~~ **Revertida el 2026-11-09**: sin EF también en proyectos existentes | Se decidió seguir [[Reglas para crear proyectos]] §3.3 | [[Itzamna.Dionisio]] |
| 2026-11-08 | Los **límites de validación los define la API**; el front copia los mismos números y lo comenta en el código | Una sola fuente de verdad; la plantilla decide cuántos dibuja | [[Itzamna.Clio]] · [[Itzamna.Dionisio]] |
| 2026-11-08 | Front: **Vite** como servidor de desarrollo y build (Vite simple, no electron-vite); sin import map ni `vendor.mjs` | Recarga en caliente, librerías directo de `node_modules`, CSP sin hash a mano, camino a TypeScript/React. `main.js` y `preload.js` no cambian | [[Ocelotl.Iris]] |
| — | Front: callbacks `Acciones` en lugar de `data-accion` / `data-valor` | Sin textos mágicos ni `switch`; los valores viajan con su tipo y se puede usar "ir a definición" | [[Ocelotl.Iris]] |
| — | Vertical slices con puertos solo en los bordes (no hexagonal por capas) | Pocas funcionalidades verticales; un slice se lee de una vez | [[Ocelotl.Hermes]] |
| — | Cada pantalla / slice es autocontenido (sus endpoints, su lógica) | Se borra una carpeta sin romper otra | Iris y Hermes |
| 2026-11-08 | La configuración obligatoria (incluido el PAT) se valida con `ValidateOnStart`. **Dónde va el secreto lo decide la forma de publicar**: si se publica copiando la carpeta (FileZilla), va en `appsettings.json`; user-secrets / variables de entorno solo si el servidor las usa | Se probó mover el PAT de ReporteriaAzure a user-secrets y se regresó: su publicación es por FileZilla | [[ReporteriaAzure]] |
| 2026-11-08 | Un servicio externo caído o sin acceso responde **502/503** con su `codigo`, no 500 | El cliente distingue "esta API falló" de "Azure DevOps falló o el token caducó" | [[ReporteriaAzure]] |
| 2026-11-09 | Con **funciones o stored procedures** en la base, la regla se mantiene: un `.sql` que las llama (`SELECT * FROM esquema.sp_x(@A)` / `CALL ...`) + la misma interfaz por consulta | Contrato por llamada y cero SQL en C#; cerraba el caso que las reglas dejaban pendiente | [[Pochteca.Nomisma]] |
| 2026-11-09 | Si Infrastructure no referencia Application, las **excepciones con `codigo` van en el proyecto de dominio (Core)** | Las lanzan tanto los handlers como los adaptadores (pasarelas) | [[Pochteca.Nomisma]] |
| 2026-11-09 | Un **rechazo de la pasarela** (tarjeta declinada) es un resultado, no un error; llaves inválidas, 5xx o timeout sí son errores (502/503) | Una caída real no debe verse como "tarjeta rechazada" | [[Pochteca.Nomisma]] |
| 2026-11-09 | Lo que se guarda **después de un efecto externo** (cobro, cuenta creada) usa `CancellationToken.None` | Que el cliente se vaya no deja un cobro sin registro | [[Pochteca.Nomisma]] |
| 2026-11-09 | Un proyecto con **MediatR** lo conserva (como el mediador de Itzamna); no se migra a slices solo por la regla | El cambio tocaría todos los casos de uso sin ganancia visible. Revisar la licencia de MediatR 13+ | [[Pochteca.Nomisma]] |
| 2026-11-09 | Se agregan las [[Reglas para crear proyectos]]: la arquitectura se **evalúa y documenta** por proyecto en `ARQUITECTURA.md` | Elegir capas por el problema, no por costumbre. Choques resueltos: `.sql` siempre, mapeo a mano, secretos según la forma de publicar, front JS en dos variantes (sin bundler / Vite) | Todos |

## Cómo se mantiene
- Cuando se tome una decisión de estructura nueva, se agrega a la tabla de **Decisiones** y a la nota de reglas que corresponda.
- Lo aprendido en un proyecto concreto va en su nota de *Aprendizajes*; lo que sirva para **todos** los proyectos se sube aquí.
