---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Automatización, DevOps y operación
tags: [automatizacion-devops]
contenido: completo
---
# Scripts con bash

Parte de: [[08-Automatizacion-DevOps/01 Automatización y CICD/_Índice Automatización y CICD|Automatización y CICD]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**bash** es el intérprete de comandos (*shell*) más común en Linux y el que usan por defecto los agentes `ubuntu-latest` de [[Azure Pipelines]] y [[GitHub Actions]], los contenedores y WSL en Windows. Un script de bash es un archivo de texto con comandos que se ejecutan en orden, con variables, condiciones y ciclos. Sirve para pegar herramientas entre sí: `curl`, `grep`, `az`, `dotnet`, `zip`, `sqlcmd`.

### Cómo funciona
**Encabezado y modo estricto.** Empieza siempre con:

```bash
#!/usr/bin/env bash
set -euo pipefail
```

- `-e`: el script **se detiene** si un comando devuelve un código distinto de 0. Sin esto, bash sigue ejecutando como si nada después de un error.
- `-u`: usar una variable **no definida** es error (atrapa typos como `$ARCHVO`).
- `-o pipefail`: en un *pipe* (`cmd1 | cmd2`), si cualquiera falla, el pipe completo falla. Sin esto, solo cuenta el último comando.

**Códigos de salida.** Cada comando devuelve un número: `0` es éxito, cualquier otro es error. Se consulta con `$?` justo después del comando, o directamente con `if comando; then ...`. Tu script debe terminar con `exit 0` o `exit 1` para que el pipeline sepa si falló.

**Comillas.** Bash separa las variables sin comillas por espacios y expande comodines. Si `archivo="reporte final.log"`, entonces `rm $archivo` intenta borrar `reporte` y `final.log`. Con `rm "$archivo"` se trata como un solo argumento. Regla: **todas las variables entre comillas dobles**, salvo que quieras explícitamente la separación.

Otras piezas útiles: `$1`, `$2` (argumentos), `${1:?mensaje}` (argumento obligatorio), `$(comando)` (capturar salida), `[ ... ]` o `test` (comparaciones; bash también tiene la forma extendida de doble corchete), `for`, `while`, funciones y `>&2` (escribir a la salida de error).

### Ejemplo
**Reintentar hasta obtener 200** (útil después de desplegar, mientras la app arranca):

```bash
#!/usr/bin/env bash
set -euo pipefail
url="${1:?Uso: $0 <url>}"
intentos=5
espera=10
for ((i = 1; i <= intentos; i++)); do
  # || true: un fallo de red no debe matar el script por set -e
  codigo=$(curl -s -o /dev/null -w "%{http_code}" --max-time 5 "$url" || true)
  if [ "$codigo" = "200" ]; then
    echo "OK en el intento $i"
    exit 0
  fi
  echo "Intento $i/$intentos: HTTP $codigo; reintento en ${espera}s" >&2
  (( i < intentos )) && sleep "$espera"
done
echo "Falló después de $intentos intentos" >&2
exit 1
```

**Contar líneas con `ERROR` en cada `.log`**:

```bash
for archivo in *.log; do
  # grep -c imprime 0 y sale con 1 si no hay coincidencias; || true evita que set -e corte
  printf '%s: %s\n' "$archivo" "$(grep -c 'ERROR' "$archivo" || true)"
done
# Versión corta: grep -c ERROR -- *.log
```

**Comprimir `publish` con la fecha en el nombre**:

```bash
fecha=$(date +%F)                 # 2026-11-09
zip -qr "app-${fecha}.zip" publish
echo "Generado app-${fecha}.zip"
```

**Saber si el comando anterior falló**:

```bash
if ! dotnet test -c Release; then
  echo "Las pruebas fallaron" >&2
  exit 1
fi
# o bien, sin set -e:
dotnet build; rc=$?
[ "$rc" -ne 0 ] && echo "build devolvió $rc"
```

En un pipeline YAML de Azure DevOps, un paso `- bash: ./scripts/reintentar.sh "$(UrlApp)/health"` marca el paso como fallido si el script sale con código distinto de 0.

### Cuándo usarlo y cuándo no
- **Úsalo** para pasos de CI en Linux, pegar comandos de CLI, tareas cortas en contenedores y servidores.
- **Prefiere [[Scripts con PowerShell]]** si el destino es Windows o trabajas mucho con objetos de Azure.
- **Prefiere [[Scripts con Python]]** cuando aparecen estructuras de datos (JSON complejo, diccionarios), más de ~100 líneas o necesitas pruebas.

### Errores comunes
1. **Olvidar `set -euo pipefail`**: el script «pasa» aunque un paso intermedio falló.
2. **Variables sin comillas**: rompe con espacios en rutas, muy comunes en Windows/WSL.
3. **Fin de línea CRLF** al editar en Windows: `bash\r: No such file or directory`. Configura `.gitattributes` con `*.sh text eol=lf`.
4. **Permiso de ejecución**: `chmod +x script.sh` y en Git `git update-index --chmod=+x`.
5. **Imprimir secretos** con `set -x` activo.

### Para profundizar
- [Manual de GNU Bash](https://www.gnu.org/software/bash/manual/bash.html)
- [ShellCheck: linter para scripts de shell](https://www.shellcheck.net/)
- [Azure Pipelines: tarea Bash](https://learn.microsoft.com/azure/devops/pipelines/tasks/reference/bash-v3)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Escribe un script que reciba una URL y reintente 5 veces cada 10 s hasta obtener 200.
> ```bash
> #!/usr/bin/env bash
> set -euo pipefail
> url="$1"
> for i in {1..5}; do
>   code=$(curl -s -o /dev/null -w '%{http_code}' "$url" || true)
>   [ "$code" = "200" ] && { echo "ok"; exit 0; }
>   echo "intento $i: $code"; sleep 10
> done
> exit 1
> ```

> [!question]- Cuenta cuántas líneas con `ERROR` hay en cada `.log` de la carpeta actual.
> ```bash
> for f in *.log; do
>   echo "$f: $(grep -c 'ERROR' "$f" || true)"
> done
> ```
> (`grep -c` sale con 1 si cuenta 0; `|| true` evita que `set -e` corte el bucle.)

> [!question]- Comprime la carpeta `publish` en un zip con la fecha en el nombre (`app-2026-11-09.zip`).
> ```bash
> zip -r "app-$(date +%F).zip" publish
> ```

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace `set -euo pipefail`?
> `-e`: termina ante un comando que falla. `-u`: error si usas una variable no definida. `-o pipefail`: un pipeline falla si falla cualquier comando, no solo el último.

> [!question]- ¿Por qué poner variables entre comillas (`"$archivo"`)?
> Sin comillas, un valor con espacios se divide en varias palabras y los comodines se expanden: `rm $archivo` con «mis datos.txt» intentaría borrar dos archivos.

> [!question]- ¿Cómo sabes si el comando anterior falló?
> Con su código de salida en `$?` (0 = éxito) o directamente `if comando; then ...; fi`. Tus scripts deben terminar con `exit 1` ante error para que el pipeline lo note.

