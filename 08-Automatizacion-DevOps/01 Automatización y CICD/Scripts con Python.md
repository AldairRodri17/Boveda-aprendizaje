---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Automatización, DevOps y operación
tags: [automatizacion-devops]
---
# Scripts con Python

Parte de: [[08-Automatizacion-DevOps/01 Automatización y CICD/_Índice Automatización y CICD|Automatización y CICD]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Python es un lenguaje de propósito general muy usado para **automatización**: está instalado en los agentes de CI y en la mayoría de los Linux, tiene una librería estándar enorme (archivos, JSON, HTTP, CSV, fechas, procesos) y se lee casi como pseudocódigo. Un script de Python es un archivo `.py` que ejecutas con `python3 script.py`.

### Cómo funciona
Piezas de la librería estándar que más usarás en automatización:
- **`argparse`**: argumentos de línea de comandos con ayuda automática (`--help`), tipos y valores por defecto.
- **`pathlib`**: rutas y archivos con objetos (`Path("logs").glob("*.log")`, `p.stat().st_mtime`).
- **`json`**, **`csv`**: leer y escribir datos estructurados.
- **`collections.Counter`**: contar ocurrencias en una línea.
- **`urllib.request`**: peticiones HTTP sin instalar nada (para algo más cómodo, el paquete externo `requests` o `httpx`).
- **`subprocess.run`**: ejecutar comandos externos. Con `check=True` lanza `CalledProcessError` si el comando devuelve código distinto de 0; con `capture_output=True, text=True` capturas su salida como texto.
- **`sys.exit(código)`**: termina con un código de salida; `0` éxito, otro valor error. Así el pipeline sabe si el paso falló.

**`if __name__ == "__main__":`** Cuando ejecutas un archivo directamente, Python pone `__name__` en `"__main__"`; cuando otro archivo lo **importa**, `__name__` es el nombre del módulo. Ese `if` permite que el archivo sea a la vez **script ejecutable** y **módulo importable**: puedes importar sus funciones desde pruebas unitarias sin que se ejecute la lógica principal.

### Ejemplo
**Listar archivos más viejos que `--dias`**:

```python
"""Lista archivos de una carpeta más viejos que N días."""
import argparse
import sys
import time
from pathlib import Path


def archivos_viejos(carpeta: Path, dias: int) -> list[Path]:
    limite = time.time() - dias * 86_400
    return sorted(p for p in carpeta.iterdir() if p.is_file() and p.stat().st_mtime < limite)


def main() -> int:
    parser = argparse.ArgumentParser(description=__doc__)
    parser.add_argument("carpeta", type=Path)
    parser.add_argument("--dias", type=int, default=30, help="antigüedad mínima en días")
    args = parser.parse_args()

    if not args.carpeta.is_dir():
        print(f"No existe la carpeta {args.carpeta}", file=sys.stderr)
        return 2

    for p in archivos_viejos(args.carpeta, args.dias):
        print(p)
    return 0


if __name__ == "__main__":
    sys.exit(main())
```

Uso: `python3 viejos.py /var/log/dionisio --dias 15`.

**Contar work items por estado** desde un JSON como `[{"id":1,"estado":"Active"}, ...]`:

```python
import json
import sys
from collections import Counter

with open(sys.argv[1], encoding="utf-8") as f:
    items = json.load(f)

conteo = Counter(item["estado"] for item in items)
for estado, total in conteo.most_common():
    print(f"{estado}: {total}")
```

**Salir con código 1 si una URL no responde 200**, solo con la librería estándar:

```python
import sys
import urllib.error
import urllib.request


def responde_200(url: str, timeout: float = 5) -> bool:
    try:
        with urllib.request.urlopen(url, timeout=timeout) as resp:
            return resp.status == 200
    except (urllib.error.URLError, TimeoutError) as ex:   # HTTPError (404, 500) hereda de URLError
        print(f"Error: {ex}", file=sys.stderr)
        return False


if __name__ == "__main__":
    sys.exit(0 if responde_200(sys.argv[1]) else 1)
```

**Ejecutar un comando externo y fallar si devuelve error**:

```python
import subprocess

resultado = subprocess.run(
    ["dotnet", "test", "-c", "Release"],
    check=True,              # lanza CalledProcessError si el código no es 0
    capture_output=True,
    text=True,
)
print(resultado.stdout)
```

Pasa siempre los argumentos como **lista**, no como una cadena con `shell=True`: evitas inyección de comandos si algún valor viene de fuera.

### Cuándo usarlo y cuándo no
- **Elige Python** cuando el script crece, manipula datos (JSON, CSV, Excel con `openpyxl`), necesita pruebas o reutiliza lógica; también para llamar APIs con lógica de paginación y reintentos.
- **Prefiere [[Scripts con bash]]** para encadenar unos pocos comandos de CLI.
- **Prefiere [[Scripts con PowerShell]]** para administrar Windows, IIS o recursos de Azure con objetos.
- Si la automatización se vuelve un servicio con horario y monitoreo, muévela a una [[Azure Functions programadas|Azure Function]] (que también soporta Python).

### Errores comunes
1. **Instalar paquetes en el Python del sistema.** Usa un entorno virtual (`python3 -m venv .venv`) y un `requirements.txt`.
2. **No devolver código de salida**: el script imprime «error» pero termina con 0 y el pipeline queda verde.
3. **`shell=True` con datos externos**: riesgo de inyección.
4. **Rutas armadas con `+` y `\`**: usa `pathlib` para que funcione en Windows y Linux.
5. **Codificación**: abre archivos con `encoding="utf-8"` explícito; en Windows el valor por defecto puede ser otro.

### Para profundizar
- [Documentación oficial de Python](https://docs.python.org/3/)
- [argparse](https://docs.python.org/3/library/argparse.html)
- [subprocess](https://docs.python.org/3/library/subprocess.html)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Script que reciba `--dias` y liste archivos de una carpeta más viejos que eso.
> ```python
> import argparse, time
> from pathlib import Path
>
> p = argparse.ArgumentParser()
> p.add_argument("carpeta")
> p.add_argument("--dias", type=int, default=30)
> a = p.parse_args()
> limite = time.time() - a.dias * 86400
> for f in Path(a.carpeta).rglob("*"):
>     if f.is_file() and f.stat().st_mtime < limite:
>         print(f)
> ```

> [!question]- Lee un JSON de work items (`[{"id":1,"estado":"Active"}, ...]`) e imprime cuántos hay por estado.
> ```python
> import json, collections
> items = json.load(open("items.json", encoding="utf-8"))
> for estado, n in collections.Counter(i["estado"] for i in items).items():
>     print(estado, n)
> ```

> [!question]- Haz que el script termine con código 1 si una URL no responde 200 (solo librería estándar).
> ```python
> import sys, urllib.request
> try:
>     with urllib.request.urlopen("https://mi-api/health", timeout=10) as r:
>         sys.exit(0 if r.status == 200 else 1)
> except Exception as e:
>     print(e, file=sys.stderr); sys.exit(1)
> ```

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cuándo eliges Python en vez de bash o PowerShell?
> Cuando hay lógica más compleja: parsear JSON/CSV, llamar APIs, manejar errores con detalle o necesitas pruebas unitarias. Es portable y tiene muchas librerías.

> [!question]- ¿Cómo ejecutas un comando externo y fallas si devuelve error?
> `subprocess.run(["dotnet", "test"], check=True)`: lanza `CalledProcessError` si el código de salida no es 0. Pasa la lista de argumentos, no un string con `shell=True`.

> [!question]- ¿Para qué sirve `if __name__ == "__main__":`?
> Para que el código principal solo corra al ejecutar el archivo, no al importarlo desde otro módulo o desde las pruebas.

