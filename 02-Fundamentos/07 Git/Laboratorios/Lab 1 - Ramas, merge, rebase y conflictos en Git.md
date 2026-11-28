---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Fundamentos
herramienta: Git (Git for Windows con Git Bash) y VS Code
tags: [fundamentos, laboratorio]
contenido: completo
---
# Lab 1 - Ramas, merge, rebase y conflictos en Git

Parte de: [[02-Fundamentos/07 Git/_Índice Git|Git]]

**Cubre:** [[Git - qué es y cómo funciona]] · [[Git - commit]] · [[Git - ramas]] · [[Git - merge]] · [[Git - rebase]] · [[Git - cherry-pick]] · [[Git - stash]] · [[Git - resolver conflictos]] · [[Git - reset y revert]] · [[Git - .gitignore]] · [[Git - tags y versionado semántico]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Practicar en un repositorio local desechable los casos que más asustan: merge con y sin fast-forward, conflictos, rebase, cherry-pick, stash y recuperar trabajo con `reflog`.

## Requisitos
Git 2.23+ (para `git switch`/`git restore`). Usa Git Bash o PowerShell; para ver la historia: `git log --oneline --graph --all`.

## Pasos
1. Crea una carpeta `lab-git`, `git init -b main` y un archivo `calc.txt` con dos líneas: `precio = 100` e `iva = 0.16`. Haz el primer commit. Agrega un `.gitignore` que ignore `*.log` y verifica con `git status` que `debug.log` no aparece.
2. **Fast-forward**: crea `docs/readme`, agrega `README.md`, commit, regresa a `main` y haz merge. Observa la salida.
3. **Merge de tres vías**: crea `feature/descuento` y agrega al final `descuento = 0.10`. En `main` cambia `precio = 120`. Haz merge de la rama en `main`. Mira el grafo.
4. **Conflicto en merge**: crea `feature/iva-frontera` desde `main` y cambia la línea del IVA a `iva = 0.08`. En `main` cambia la misma línea a `iva = 0.16 # general`. Haz merge, resuelve dejando `iva = 0.16 # general` y agrega una línea `iva_frontera = 0.08`. Termina el merge.
5. **Rebase**: crea `feature/redondeo` con dos commits (dos líneas nuevas distintas). Mientras, en `main` haz un commit que modifique `README.md`. Anota los hashes de tu rama, haz `git rebase main` desde `feature/redondeo` y vuelve a anotarlos.
6. **Conflicto en rebase**: repite la idea del paso 4 pero integrando con `rebase` en vez de `merge`. Resuelve y usa `git rebase --continue`.
7. **Cherry-pick**: en una rama `experimento` haz 3 commits; lleva solo el segundo a `main` con `git cherry-pick -x`.
8. **Stash**: con cambios sin commit en `calc.txt` y un archivo nuevo, intenta cambiar de rama; guarda todo con stash (incluido el archivo nuevo), cambia, regresa y recupéralo.
9. **Reset, reflog y revert**: en `main` haz `git reset --hard HEAD~2`, recupera los commits con `git reflog`. Después deshaz un commit con `git revert`.
10. **Tag**: crea el tag anotado `v1.0.0` y muéstralo con `git show v1.0.0`.

## Criterios de terminado
- [ ] El paso 2 dice `Fast-forward` y no crea commit de merge.
- [ ] El paso 3 crea un commit de merge con **dos padres** (`git log --graph` lo muestra).
- [ ] En el paso 4 viste los marcadores `<<<<<<<`, `=======`, `>>>>>>>` y el archivo final no los tiene.
- [ ] En el paso 5 los hashes de tus commits cambiaron tras el rebase y la historia quedó lineal.
- [ ] En `main` aparece solo el segundo commit de `experimento`, con la línea `(cherry picked from commit ...)`.
- [ ] `git stash list` queda vacío al final y recuperaste el archivo nuevo.
- [ ] Recuperaste los commits borrados con `reset --hard` usando `reflog`.
- [ ] `git tag -n` muestra `v1.0.0` con su mensaje.

## Solución

> [!success]- Pasos 1–3
> ```bash
> mkdir lab-git && cd lab-git && git init -b main
> printf 'precio = 100\niva = 0.16\n' > calc.txt
> echo '*.log' > .gitignore && echo x > debug.log
> git add . && git commit -m "feat: calculo inicial"
> git switch -c docs/readme && echo "# Lab" > README.md && git add . && git commit -m "docs: agrega readme"
> git switch main && git merge docs/readme          # Fast-forward
> git switch -c feature/descuento && echo 'descuento = 0.10' >> calc.txt && git commit -am "feat: descuento"
> git switch main && sed -i 's/precio = 100/precio = 120/' calc.txt && git commit -am "fix: precio"
> git merge feature/descuento                        # Merge made by the 'ort' strategy
> git log --oneline --graph --all
> ```
> Los cambios no se tocan (línea 1 vs. línea agregada al final), por eso Git combina solo, pero crea un commit de merge.

> [!success]- Paso 4: resolver el conflicto
> ```text
> <<<<<<< HEAD
> iva = 0.16 # general
> =======
> iva = 0.08
> >>>>>>> feature/iva-frontera
> ```
> Edita para dejar `iva = 0.16 # general` y `iva_frontera = 0.08`, luego `git add calc.txt` y `git commit` (Git propone el mensaje de merge).

> [!success]- Pasos 5–6: rebase
> `git switch feature/redondeo && git rebase main`. Los commits se reaplican sobre la punta de `main` con **hashes nuevos**. En un conflicto, Git se detiene en el commit que no aplica: resuelve, `git add` y `git rebase --continue` (o `--abort` para volver). Recuerda que durante un rebase `HEAD`/`ours` es la base (`main`) y `theirs` es tu commit.

> [!success]- Pasos 7–10
> ```bash
> git log --oneline experimento        # copia el hash del 2.º commit
> git switch main && git cherry-pick -x <hash>
> git stash -u && git switch otra-rama && git switch - && git stash pop
> git reset --hard HEAD~2 && git reflog && git reset --hard HEAD@{1}
> git revert <hash> --no-edit
> git tag -a v1.0.0 -m "Primera versión" && git show v1.0.0 && git tag -n
> ```
> `git stash` sin `-u` no habría guardado el archivo nuevo. En el reflog, `HEAD@{1}` es a donde apuntaba `HEAD` antes del reset.

## Notas personales

_Qué me costó, errores que cometí, dudas._
