---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Fundamentos
tags: [fundamentos]
---
# Git - stash

Parte de: [[02-Fundamentos/07 Git/_Índice Git|Git]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`git stash` guarda temporalmente los cambios que todavía no quieres confirmar y deja tu directorio de trabajo limpio, como estaba en el último commit. Sirve para **cambiar de contexto rápido**: atender un bug urgente, hacer `pull` o cambiar de rama sin perder lo que llevas a medias y sin crear un commit "WIP".

### Cómo funciona
El stash es una **pila** (la más reciente arriba, `stash@{0}`). Cada entrada se guarda internamente como commits especiales: uno con el estado del staging y otro con el del directorio de trabajo (y un tercero con archivos no rastreados si usas `-u`).

```bash
git stash push -m "filtro de fechas a medias"   # guarda (push es el subcomando por defecto)
git stash list                                   # stash@{0}: On feature/...: filtro de fechas a medias
git stash show -p stash@{0}                      # qué contiene (como diff)
git stash pop                                    # aplica el último y lo borra de la pila
git stash apply stash@{1}                        # aplica uno pero lo conserva en la pila
git stash drop stash@{1}                         # borra uno
git stash clear                                  # borra todos (cuidado)
git stash branch fix/rescate stash@{0}           # crea rama desde donde se hizo el stash y lo aplica
```

**`pop` vs `apply`:** ambos aplican los cambios; `pop` además los elimina de la pila **si se aplicaron sin conflicto**. Si hubo conflicto, el stash se conserva para que no lo pierdas. `apply` es útil cuando quieres aplicar el mismo stash en varias ramas o no estás seguro de que va a salir bien.

**Qué guarda y qué no.** Por defecto `git stash` solo guarda cambios de **archivos rastreados** (modificados o en staging). Un archivo **nuevo que nunca agregaste** con `git add` no es rastreado, así que se queda en el directorio. Para incluirlos:

```bash
git stash push -u -m "con archivos nuevos"       # --include-untracked
git stash push -a                                # --all: incluye también ignorados (bin/, obj/...), rara vez necesario
```

También puedes guardar solo algunos archivos: `git stash push -m "solo sql" -- src/Sql/` o por bloques con `git stash push -p`.

### Ejemplo
Estás en Itzamna.Dionisio implementando un endpoint y te reportan un error en producción:

```bash
git status                       # tienes cambios en 3 archivos y un ObtenerSaldo.sql nuevo
git stash push -u -m "AB1560 endpoint saldos"
git switch main && git pull --ff-only
git switch -c hotfix/AB1561-timeout
 # ... corriges, commit, push, PR
git switch feature/AB1560-saldos
git stash pop
git status                       # todo regresó, incluido el .sql nuevo
```

Si al hacer `pop` sale conflicto (porque la rama cambió mientras tanto), resuélvelo como cualquier conflicto, haz `git add` y luego `git stash drop` manualmente, ya que en ese caso no se borra solo.

### Cuándo usarlo y cuándo no
- **Úsalo** para interrupciones cortas, para mover cambios que hiciste en la rama equivocada (`stash`, `switch`, `pop`) o para limpiar antes de un `pull`/`rebase`.
- **No lo uses como almacén de trabajo** por días: los stashes no tienen nombre de rama visible, no se suben al remoto y se olvidan. Si el trabajo va a durar, haz un commit en tu rama (puedes reescribirlo después) y súbelo.
- Alternativa para trabajar en dos cosas a la vez: `git worktree add ../Dionisio-hotfix main` te da otra carpeta con otra rama, sin stash.

### Errores comunes
- **Creer que se guardó un archivo nuevo** y luego borrar la carpeta: sin `-u`, no estaba en el stash. Revisa `git stash show --include-untracked` o usa siempre `-u`.
- **Acumular stashes sin mensaje**: "WIP on main: 3a8b4c1" no dice nada. Usa `-m`.
- **`git stash clear` o `drop` por error**: los commits del stash quedan sin referencia; a veces se recuperan con `git fsck --unreachable | grep commit`, pero no cuentes con ello.
- **Aplicar en la rama equivocada**: revisa `git branch --show-current` antes de `pop`.
- **Esperar que el stash viaje con `push`**: es local; otra máquina no lo ve.

### Para profundizar
- [Referencia de git stash](https://git-scm.com/docs/git-stash)
- [Pro Git: guardado rápido y limpieza](https://git-scm.com/book/es/v2/Herramientas-de-Git-Guardado-r%C3%A1pido-y-Limpieza)
- [Referencia de git worktree](https://git-scm.com/docs/git-worktree)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Para qué sirve `git stash`?
> Para guardar temporalmente cambios sin commit y dejar el directorio limpio (por ejemplo para cambiar de rama de urgencia).

> [!question]- ¿Qué diferencia hay entre `git stash pop` y `git stash apply`?
> `apply` aplica y **conserva** la entrada; `pop` la aplica y la borra si no hubo conflicto.

> [!question]- ¿Por qué `git stash` no guardó un archivo nuevo?
> Por defecto no incluye archivos sin seguimiento. Usa `git stash -u` (o `--include-untracked`).

## Práctica

- [[Lab 1 - Ramas, merge, rebase y conflictos en Git]]

