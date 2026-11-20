---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Fundamentos
tags: [fundamentos]
---
# Git - hooks y pre-commit

Parte de: [[02-Fundamentos/07 Git/_Índice Git|Git]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Los **hooks de Git** son scripts que Git ejecuta automáticamente en ciertos momentos: antes de crear un commit, al escribir el mensaje, antes de un push, después de un checkout, etc. Si un hook "de antes" (pre-) termina con código distinto de cero, **Git cancela la operación**. Sirven para automatizar revisiones rápidas: formato, lint, pruebas veloces, mensajes de commit con formato, detección de secretos.

### Cómo funciona
Los hooks viven por defecto en `.git/hooks/`. Git trae ejemplos con extensión `.sample`; para activar uno, creas un archivo con el nombre exacto del hook y lo haces ejecutable. Los más usados del lado del cliente:

| Hook | Cuándo corre | Uso típico |
|---|---|---|
| `pre-commit` | antes de crear el commit | formato, lint, buscar secretos o marcadores de conflicto |
| `prepare-commit-msg` | antes de abrir el editor | agregar el número de work item según la rama |
| `commit-msg` | después de escribir el mensaje | validar [[Conventional Commits]] |
| `pre-push` | antes de enviar al remoto | correr pruebas rápidas |
| `post-checkout`, `post-merge` | después de cambiar de rama o integrar | avisar que hay que restaurar paquetes |

**Por qué no se comparten al clonar:** la carpeta `.git/` **no forma parte del repositorio versionado**; `git clone` no copia hooks. Es una decisión de seguridad: si clonar instalara scripts que se ejecutan solos, cualquier repositorio podría correr código en tu máquina.

**Cómo se resuelve:**
1. Versionar los scripts en una carpeta del repo (por ejemplo `.githooks/`) y que cada quien ejecute una vez `git config core.hooksPath .githooks`.
2. Usar un **gestor de hooks** que los instale con un comando:
   - **Husky** (ecosistema Node): se activa con el script `prepare` de `package.json`, que corre al hacer `npm install`.
   - **pre-commit** (framework en Python, multi-lenguaje): un archivo `.pre-commit-config.yaml` describe los hooks y `pre-commit install` los instala.
   - **Husky.Net**: una herramienta .NET con la misma idea, instalable como herramienta local de `dotnet`.

### Ejemplo
Repositorio de Itzamna.Dionisio con hooks versionados.

`.githooks/pre-commit` (bash; en Windows corre con el bash que trae Git for Windows):

```bash
#!/usr/bin/env bash
set -euo pipefail

 # 1. Formato de C#: solo verifica, no modifica
dotnet format --verify-no-changes --verbosity quiet || {
  echo "Formato incorrecto. Ejecuta: dotnet format"; exit 1; }

 # 2. Evita marcadores de conflicto olvidados en lo que está en staging
if git diff --cached --check | grep -q .; then
  echo "Hay espacios sobrantes o marcadores de conflicto"; exit 1
fi

 # 3. Evita subir archivos de configuración local
if git diff --cached --name-only | grep -E 'appsettings\..*Local\.json|\.env$'; then
  echo "No subas configuración local ni .env"; exit 1
fi
```

Activación (una vez por clon, y documentada en el README):

```bash
git config core.hooksPath .githooks
chmod +x .githooks/pre-commit      # en Linux/macOS
```

Con el framework pre-commit, el equivalente declarativo:

```yaml
 # .pre-commit-config.yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v6.0.0   # fija siempre una versión concreta; actualízala con pre-commit autoupdate
    hooks:
      - id: check-merge-conflict
      - id: detect-private-key
      - id: trailing-whitespace
```

Para saltarse los hooks existe `git commit --no-verify`, y por eso mismo **los hooks locales no sustituyen la validación en CI**: son opcionales, cada quien puede no instalarlos o desactivarlos, y pueden variar entre máquinas. Son retroalimentación **rápida**; la garantía la da el pipeline que corre en cada PR (build, pruebas, análisis, escaneo de secretos) junto con las políticas de rama.

### Cuándo usarlo y cuándo no
- **Sí**: revisiones que tarden **segundos** y eviten vueltas en el PR: formato, lint del archivo modificado, mensajes de commit, secretos obvios.
- **No**: pruebas completas de integración o builds largos en `pre-commit`; la gente terminará usando `--no-verify`. Si algo tarda minutos, ponlo en `pre-push` o déjalo al pipeline.
- Del lado del servidor, Azure Repos no permite hooks personalizados como `pre-receive`; ahí se usan **políticas de rama** y validaciones de build.

### Errores comunes
- **Hooks lentos**: el equipo los desactiva. Mide y limita a los archivos en staging.
- **Esperar que lleguen al clonar**: documenta el paso de instalación o automatízalo (Husky con `npm install`).
- **Problemas de fin de línea y permisos**: un script con CRLF o sin bit de ejecución falla en Linux/macOS. Usa `.gitattributes` (`*.sh text eol=lf`) y `git update-index --chmod=+x`.
- **Hooks que modifican archivos sin volver a agregarlos**: el commit queda con la versión vieja. Mejor verificar y fallar, o usar herramientas que lo manejen (lint-staged).
- **Confiar solo en el hook**: replica cada regla importante en CI.

### Para profundizar
- [Pro Git: puntos de enganche en Git](https://git-scm.com/book/es/v2/Personalizaci%C3%B3n-de-Git-Puntos-de-enganche-en-Git)
- [Referencia de githooks](https://git-scm.com/docs/githooks)
- [Framework pre-commit](https://pre-commit.com/)
- [Husky](https://typicode.github.io/husky/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué son los hooks de Git?
> Scripts en `.git/hooks/` que Git ejecuta en eventos (`pre-commit`, `commit-msg`, `pre-push`). Si terminan con código distinto de 0, cancelan la operación.

> [!question]- ¿Por qué los hooks no se comparten al clonar y cómo se resuelve?
> `.git/hooks` no se versiona. Se versionan en una carpeta del repo y se configura `git config core.hooksPath .githooks`, o se usan herramientas como **pre-commit** (Python), **Husky** (Node) o **Husky.Net**.

> [!question]- ¿Los hooks locales sustituyen la validación en CI?
> No. Se pueden saltar (`git commit --no-verify`). Son retroalimentación rápida; la validación obligatoria va en el pipeline y en las políticas de rama.

