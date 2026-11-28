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
# Git - .gitignore

Parte de: [[02-Fundamentos/07 Git/_Índice Git|Git]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`.gitignore` es un archivo de texto con **patrones de rutas que Git debe ignorar**: no aparecen como "untracked" en `git status` y `git add .` no los agrega. Sirve para dejar fuera del repositorio lo que se genera (compilados, dependencias), lo que es de cada máquina (configuración del IDE) y, sobre todo, lo que **no debe publicarse** (secretos, archivos `.env` locales).

### Cómo funciona
Git lee patrones de varias fuentes, de mayor a menor prioridad: el `.gitignore` de cada carpeta (aplica a esa carpeta y sus hijas), `.git/info/exclude` (local, no se comparte) y el archivo global de `core.excludesFile` (por ejemplo, para `.DS_Store` o `Thumbs.db` de tu máquina).

Reglas de los patrones:

| Patrón | Significado |
|---|---|
| `*.log` | cualquier archivo `.log` en cualquier nivel |
| `build/` | cualquier **directorio** llamado `build`, en cualquier nivel (la `/` final indica "solo directorios") |
| `/build` | solo `build` en la **raíz** del directorio donde está el `.gitignore` (la `/` inicial ancla) |
| `docs/*.pdf` | PDFs directamente en `docs/` (con una `/` en medio, el patrón queda anclado) |
| `**/temp` | `temp` en cualquier profundidad |
| `!archivo` | **negación**: vuelve a incluir algo que una regla anterior excluyó |
| `#` | comentario |

El orden importa: la última regla que coincide gana. Una limitación: **no puedes re-incluir un archivo si ignoraste su carpeta padre completa**, porque Git ni siquiera entra a la carpeta. Por ejemplo, con `logs/` y luego `!logs/conservar.txt` no funciona; hay que escribir `logs/*` y luego `!logs/conservar.txt`.

Para depurar qué regla aplica:

```bash
git check-ignore -v src/Api/obj/Debug/net10.0/Api.dll
 # .gitignore:12:[Oo]bj/	src/Api/obj/Debug/net10.0/Api.dll
```

Muestra archivo, número de línea y patrón responsables.

**Muy importante:** `.gitignore` solo afecta archivos **no rastreados**. Si un archivo ya estaba en el repositorio (alguna vez hiciste commit de él), agregarlo al `.gitignore` no hace nada; Git lo sigue rastreando. Hay que sacarlo del índice sin borrarlo del disco:

```bash
git rm --cached appsettings.Local.json
git commit -m "chore: deja de versionar appsettings.Local.json"
```

### Ejemplo
Base para una solución .NET 10 con front React + Vite (Itzamna.Dionisio + Itzamna.Clio). Lo más práctico es partir de la plantilla oficial: `dotnet new gitignore` genera un `.gitignore` completo para .NET.

```gitignore
#.NET
[Bb]in/
[Oo]bj/
*.user
.vs/
TestResults/
*.trx

#Node / Vite
node_modules/
dist/
*.local

#Configuración local y secretos
.env
.env.*
!.env.example
appsettings.*.Local.json

#Logs: ignora todos menos el de ejemplo
*.log
!ejemplo.log

#IDE / sistema
.idea/
*.swp
```

`!.env.example` permite versionar una plantilla sin valores reales para que el equipo sepa qué variables existen. Los secretos de desarrollo en .NET van mejor en [[Secretos con User Secrets]], que viven fuera del repositorio.

**Si subiste una contraseña por error**, borrarla en un commit nuevo **no basta**: sigue en la historia, en cada clon y quizá en forks o cachés. Lo correcto es:
1. **Revocar o rotar** el secreto de inmediato (cambiar la contraseña, regenerar la llave en Azure). Esto es lo único que realmente te protege.
2. Si además quieres limpiarla de la historia, reescríbela con `git filter-repo` o BFG, fuerza el push y pide a todos que vuelvan a clonar. En plataformas como GitHub también hay que pedir que se purguen cachés.
3. Prevenir: escaneo de secretos en el pipeline o en un hook (ver [[Git - hooks y pre-commit]]).

### Cuándo usarlo y cuándo no
- Crea el `.gitignore` **antes del primer commit**; es mucho más fácil que limpiar después.
- Ignora lo **reproducible** (se regenera con `dotnet build` o `npm ci`) y lo **personal**.
- **No ignores** `package-lock.json` ni `packages.lock.json` si los usas: fijan versiones y hacen los builds reproducibles.
- Preferencias de tu editor que solo tú usas van en el ignore global, no en el del repo.

### Errores comunes
- **Esperar que ignore archivos ya rastreados**: usa `git rm --cached`.
- **Ignorar la carpeta y luego intentar re-incluir un archivo dentro**: usa `carpeta/*` + `!carpeta/archivo`.
- **Confundir `build/` con `/build`**: el primero aplica en cualquier nivel, el segundo solo en la raíz.
- **Creer que ignorar equivale a proteger**: si un secreto ya llegó al remoto, considéralo comprometido.
- **Plantillas a medias**: olvidar `.vs/` o `*.user` llena los PR de ruido.

### Para profundizar
- [Referencia de gitignore](https://git-scm.com/docs/gitignore)
- [Plantillas oficiales de .gitignore (github/gitignore)](https://github.com/github/gitignore)
- [Eliminar datos confidenciales de un repositorio (GitHub Docs)](https://docs.github.com/es/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Escribe un patrón que ignore todos los `.log` excepto `importante.log`.
> ```
> *.log
> !importante.log
> ```

> [!question]- ¿Qué diferencia hay entre `build/` y `/build` en `.gitignore`?
> `build/` ignora cualquier directorio `build` en cualquier nivel; `/build` solo el de la raíz del repositorio.

> [!question]- ¿Cómo averiguas qué regla ignora `obj/x.dll`?
> `git check-ignore -v obj/x.dll`

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué va en un `.gitignore` de un proyecto .NET?
> `bin/`, `obj/`, `.vs/`, `*.user`, `appsettings.*.local.json`, `.env` y artefactos de pruebas. `dotnet new gitignore` genera uno completo.

> [!question]- Agregaste `secretos.json` al `.gitignore` pero sigue apareciendo en cambios. ¿Por qué?
> `.gitignore` solo afecta a archivos **sin seguimiento**. Hay que sacarlo del índice: `git rm --cached secretos.json` y commit.

> [!question]- Subiste por error una contraseña al repositorio. ¿Basta con borrarla en un commit nuevo?
> No: sigue en la historia. **Rota la credencial de inmediato** y, si hace falta, limpia la historia (`git filter-repo` o BFG) y coordina con el equipo.

## Práctica

- [[Lab 1 - Ramas, merge, rebase y conflictos en Git]]

