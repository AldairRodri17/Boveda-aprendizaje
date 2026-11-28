---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Fundamentos
tags: [fundamentos]
contenido: completo
---
# Git - commit

Parte de: [[02-Fundamentos/07 Git/_Índice Git|Git]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **commit** es una foto inmutable del proyecto en un momento dado, con metadatos que explican quién, cuándo y por qué. Es la unidad básica de la historia: se revisa, se revierte, se cherry-pickea y se etiqueta por commit. Un buen historial de commits es documentación viva del proyecto.

### Cómo funciona
Cada commit contiene:

- Un apuntador al **tree raíz**, es decir, el estado completo de todos los archivos versionados.
- Uno o más **padres** (cero en el primer commit, dos o más en un merge).
- **Autor** (quién escribió el cambio) y **committer** (quién lo aplicó), cada uno con nombre, correo y fecha.
- El **mensaje**.

Su identificador es el hash de todo eso. Como el hash incluye al padre, cambiar cualquier cosa de un commit (aunque sea el mensaje) produce **otro commit con otro hash**, y todos los que dependían de él también cambian. Por eso se dice que la historia publicada no debe reescribirse.

El flujo típico:

```bash
git status                 # qué cambió
git diff                   # cambios aún no preparados
git add src/Features/Clientes/ObtenerCliente.sql
git add -p                 # agrega por bloques (hunks), muy útil para separar cambios
git diff --staged          # revisa exactamente lo que va a entrar
git commit -m "feat(clientes): agrega consulta por RFC"
```

**Corregir el último commit** se hace con `--amend`, que reemplaza el commit anterior por uno nuevo:

```bash
 # Se te olvidó un archivo
git add src/Features/Clientes/ObtenerClienteValidator.cs
git commit --amend --no-edit

 # Solo quieres cambiar el mensaje
git commit --amend -m "feat(clientes): agrega consulta y validación por RFC"
```

Si ese commit ya estaba en el remoto, `--amend` reescribe historia: solo hazlo en tu rama personal y publica con `git push --force-with-lease`.

### Ejemplo
Un mensaje bien escrito para un cambio en Itzamna.Dionisio:

```text
fix(pedidos): evita duplicar pedidos al reintentar el POST

El cliente de Clio reintenta tras un timeout y el procedimiento
usp_Pedido_Insertar no validaba el folio, así que se creaban dos
registros. Ahora el procedimiento revisa el folio dentro de la
transacción y el controller responde 409 con ProblemDetails
(codigo = PEDIDO_DUPLICADO).

Refs: AB#1532
```

Lo que lo hace bueno:
- **Asunto corto** (alrededor de 50 caracteres, máximo ~72), en imperativo o presente ("agrega", "evita"), sin punto final.
- **Línea en blanco** y luego un **cuerpo** que explica el *por qué* y el contexto; el *qué* ya lo muestra el diff.
- Referencia al work item (en Azure DevOps, `AB#1532` liga el commit al elemento cuando el repo está conectado a Boards).
- Un **solo propósito**: este commit no trae además un refactor ni cambios de formato.

Para dar estructura uniforme y automatizar versiones, muchos equipos usan [[Conventional Commits]].

### Cuándo usarlo y cuándo no
- Haz commits **pequeños y frecuentes**, cada uno compilando y con pruebas pasando cuando sea posible; eso permite usar `git bisect` y revertir con precisión.
- No hagas commit de archivos generados (`bin/`, `obj/`, `node_modules/`) ni de secretos; configúralo en [[Git - .gitignore]].
- Evita los commits "WIP" en ramas compartidas; si necesitas guardar algo a medias para cambiar de contexto, usa [[Git - stash]] o haz commits locales y luego júntalos antes del PR.

### Errores comunes
- **`git commit -am` sin revisar**: agrega todos los archivos modificados (no los nuevos) y puede colar cambios de depuración. Usa `git diff --staged` antes.
- **Mensajes vacíos de significado** ("cambios", "fix", "ya quedó"): dentro de seis meses nadie sabrá qué pasó. Escribe para tu yo futuro.
- **Amend de un commit ya publicado** y luego `push --force` sin lease: pisa el trabajo de otros. Usa `--force-with-lease` y solo en ramas tuyas.
- **Identidad mal configurada**: commits con el correo personal en el repo del trabajo. Revisa `git config user.email` por repositorio.
- **Mezclar formato y lógica**: un reformateo de 300 líneas esconde el cambio real de 3; sepáralos en commits distintos.

### Para profundizar
- [Pro Git: guardando cambios en el repositorio](https://git-scm.com/book/es/v2/Fundamentos-de-Git-Guardando-cambios-en-el-Repositorio)
- [Referencia de git commit](https://git-scm.com/docs/git-commit)
- [Vincular commits a work items en Azure Boards](https://learn.microsoft.com/azure/devops/boards/backlogs/connect-work-items-to-git-dev-ops)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué contiene un commit?
> Una instantánea del proyecto (árbol), referencia a su(s) padre(s), autor, fecha y mensaje. Su hash depende de todo eso, por eso cambiar un commit crea otro distinto.

> [!question]- ¿Cómo corriges el último commit (mensaje o un archivo olvidado)?
> `git add archivo` y `git commit --amend`. Solo si **no lo has publicado**; si ya está en el remoto, reescribirlo obliga a `push --force` y afecta a otros.

> [!question]- ¿Qué hace un buen mensaje de commit?
> Línea de resumen corta en imperativo ("Corrige cálculo de IVA en facturas"), línea en blanco y, si hace falta, el porqué del cambio. Un commit = un cambio lógico.

## Práctica

- [[Lab 1 - Ramas, merge, rebase y conflictos en Git]]

