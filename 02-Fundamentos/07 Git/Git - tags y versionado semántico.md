---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Fundamentos
tags: [fundamentos]
---
# Git - tags y versionado semántico

Parte de: [[02-Fundamentos/07 Git/_Índice Git|Git]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **tag** es un nombre fijo que apunta a un commit, normalmente para marcar una **versión publicada** (`v1.4.0`). A diferencia de una rama, no se mueve cuando haces commits nuevos.

El **versionado semántico (SemVer 2.0.0)** es una convención para que el número de versión **comunique el tipo de cambio**: `MAYOR.MENOR.PARCHE`.

- **MAYOR**: cambios **incompatibles** con la versión anterior (quien te usa debe adaptar su código).
- **MENOR**: funcionalidad **nueva compatible** hacia atrás.
- **PARCHE**: **correcciones** compatibles, sin funcionalidad nueva.

Al subir uno, los de la derecha vuelven a cero: de `3.7.4` con una funcionalidad nueva pasas a `3.8.0`; con un cambio incompatible, a `4.0.0`.

### Cómo funciona
**Tags ligeros vs anotados:**
- **Ligero**: solo una referencia con nombre al commit, como una rama que no se mueve. `git tag v1.0.0-local`.
- **Anotado**: un **objeto** en la base de Git con autor, fecha, mensaje (y opcionalmente firma GPG/SSH). `git tag -a v1.0.0 -m "Primera versión estable"`. Es el recomendado para releases porque guarda quién y cuándo lo publicó, y `git describe` lo usa por defecto.

**Publicarlos:** `git push` **no envía tags** por defecto. Hay que hacerlo explícitamente:

```bash
git push origin v1.0.0         # un tag
git push origin --tags         # todos los locales (cuidado con tags de prueba)
git push --follow-tags         # empuja commits y los tags anotados que apuntan a ellos
```

Otros comandos: `git tag -l "v1.*"` lista, `git show v1.0.0` muestra el tag y su commit, `git tag -d v1.0.0` borra local y `git push origin --delete v1.0.0` borra en el remoto (evítalo si ya alguien lo usó).

**Pre-releases y orden (precedencia).** SemVer permite sufijos tras un guion: `2.0.0-alpha`, `2.0.0-beta.2`, `2.0.0-rc.1`. Una pre-release es **menor** que la versión normal del mismo número. Entre pre-releases se comparan los identificadores separados por puntos de izquierda a derecha: los numéricos como números y los de texto en orden ASCII; un identificador numérico va antes que uno de texto y, si todo lo anterior empata, el que tiene más identificadores es mayor. Así:

`1.9.0 < 2.0.0-alpha < 2.0.0-alpha.1 < 2.0.0-beta < 2.0.0-beta.2 < 2.0.0-beta.11 < 2.0.0-rc.1 < 2.0.0`

Los metadatos de build (`+20261109.sha.5114f85`) **no** cuentan para el orden. Además, la versión `0.y.z` indica desarrollo inicial: cualquier cosa puede cambiar y no se promete estabilidad.

### Ejemplo
Publicar una versión de la librería interna de Xolotl.Hecate y empaquetarla como NuGet:

```bash
git switch main && git pull --ff-only
git tag -a v1.3.0 -m "Agrega soporte para rotación de llaves RS256"
git push origin v1.3.0
```

En el `.csproj` (o mejor, inyectado desde el pipeline):

```xml
<PropertyGroup>
  <Version>1.3.0</Version>
</PropertyGroup>
```

Un pipeline YAML de [[Azure Pipelines]] puede dispararse solo con tags:

```yaml
trigger:
  tags:
    include:
      - v*
steps:
  - pwsh: |
      # Build.SourceBranchName vale "v1.3.0" cuando el disparador es el tag refs/tags/v1.3.0
      $version = "$(Build.SourceBranchName)".TrimStart('v')
      dotnet pack -c Release -p:Version=$version -o $(Build.ArtifactStagingDirectory)
```

Herramientas como GitVersion o semantic-release calculan la versión a partir de los tags y de [[Conventional Commits]].

### Cuándo usarlo y cuándo no
- **Tags anotados** para cada release que llega a producción o a un feed de paquetes.
- **SemVer** tiene más valor en **librerías y APIs públicas**, donde otros dependen de la compatibilidad. En una app web interna que se despliega continuamente, muchos equipos usan número de build o fecha, y reservan SemVer para la API versionada.
- No muevas ni reutilices un tag publicado: si `v1.3.0` salió mal, publica `v1.3.1`.

### Errores comunes
- **Pensar que `git push` subió el tag**: usa `git push origin <tag>` o `--follow-tags`.
- **Subir con `--tags` tags de pruebas locales**: usa nombres claros y empuja uno por uno.
- **No reiniciar a cero**: después de `1.4.7` con funcionalidad nueva viene `1.5.0`, no `1.5.7`.
- **Romper compatibilidad en una versión MENOR** (por ejemplo, renombrar un campo del JSON): rompe a los clientes que confían en SemVer.
- **Ordenar versiones como texto**: `1.10.0` es mayor que `1.9.0`; usa comparación por partes numéricas.

### Para profundizar
- [Versionado Semántico 2.0.0 (en español)](https://semver.org/lang/es/)
- [Pro Git: etiquetado](https://git-scm.com/book/es/v2/Fundamentos-de-Git-Etiquetado)
- [Referencia de git tag](https://git-scm.com/docs/git-tag)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- ¿Qué versión sigue a 2.3.9 si solo corregiste un bug?
> **2.3.10**.

> [!question]- Ordena: `1.0.0`, `1.0.0-rc.1`, `1.0.0-alpha`, `0.9.12`.
> `0.9.12` < `1.0.0-alpha` < `1.0.0-rc.1` < `1.0.0`.

> [!question]- Crea y publica el tag anotado `v1.0.0` en el commit actual.
> `git tag -a v1.0.0 -m "Primera versión estable"` y `git push origin v1.0.0`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre un tag ligero y uno anotado?
> El **ligero** es solo un nombre para un commit. El **anotado** (`git tag -a v1.2.0 -m "..."`) es un objeto con autor, fecha y mensaje, y puede firmarse. Para releases usa anotados.

> [!question]- ¿Qué significa cada número en SemVer `MAYOR.MENOR.PARCHE`?
> **MAYOR**: cambios incompatibles. **MENOR**: funcionalidad nueva compatible. **PARCHE**: correcciones compatibles. Prerelease: `2.0.0-beta.1`.

> [!question]- ¿Por qué no aparece tu tag en el remoto después de `git push`?
> Los tags no se suben por defecto. Usa `git push origin v1.2.0` o `git push --follow-tags`.

## Práctica

- [[Lab 1 - Ramas, merge, rebase y conflictos en Git]]

