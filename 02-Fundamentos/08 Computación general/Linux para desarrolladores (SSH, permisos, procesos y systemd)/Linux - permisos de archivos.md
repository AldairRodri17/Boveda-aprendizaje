---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Fundamentos
tags: [fundamentos]
---
# Linux - permisos de archivos

Parte de: [[Linux para desarrolladores (SSH, permisos, procesos y systemd)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
En Linux cada archivo y directorio tiene un **dueño** (usuario), un **grupo** y tres conjuntos de permisos: para el **dueño** (*user*), para el **grupo** (*group*) y para **todos los demás** (*others*). Cada conjunto dice si se puede **leer** (`r`), **escribir** (`w`) y **ejecutar** (`x`). Este modelo decide qué puede tocar tu aplicación y es la causa más común del error `Permission denied`.

### Cómo funciona
`ls -l` muestra los permisos como 10 caracteres:

```text
-rwxr-x---  1 dionisio apps  4096 nov  9 10:00 deploy.sh
│└┬┘└┬┘└┬┘    └──┬───┘ └┬─┘
│ │  │  │       dueño  grupo
│ │  │  └ otros: ---  (nada)
│ │  └ grupo:  r-x  (leer y ejecutar)
│ └ dueño:  rwx  (leer, escribir, ejecutar)
└ tipo: - archivo, d directorio, l enlace simbólico
```

**Notación octal:** cada permiso vale un número y se suman por grupo: `r = 4`, `w = 2`, `x = 1`.

| Simbólico | Cálculo | Octal |
|---|---|---|
| `rwx` | 4+2+1 | 7 |
| `rw-` | 4+2 | 6 |
| `r-x` | 4+1 | 5 |
| `r--` | 4 | 4 |
| `---` | 0 | 0 |

Así, `rwxr-x---` es **750** y `rw-rw-r--` es **664**. Valores típicos: `755` para directorios y ejecutables públicos, `644` para archivos de lectura general, `600` para secretos (solo el dueño lee y escribe), `700` para carpetas privadas como `~/.ssh`.

**En directorios los bits significan otra cosa:**
- `r`: **listar** los nombres de su contenido.
- `w`: **crear, borrar o renombrar** archivos dentro (aunque no seas dueño del archivo).
- `x`: **atravesarlo**: entrar con `cd` y acceder a archivos dentro si conoces su nombre. Sin `x` en un directorio, no puedes llegar a nada de lo que contiene, aunque el archivo final tenga permisos abiertos. Por eso hay que revisar **toda la ruta**.

Comandos:

```bash
chmod 640 config.json        # octal: dueño rw-, grupo r--, otros ---
chmod u+x deploy.sh          # simbólico: agrega x solo al dueño, sin tocar lo demás
chmod go-w reporte.xlsx      # quita escritura a grupo y otros
chmod -R g+rX /var/dionisio  # X mayúscula: x solo en directorios (y archivos ya ejecutables)
chown dionisio:apps archivo  # cambia dueño y grupo
umask                        # máscara que define permisos de archivos nuevos (p. ej. 0022)
```

La forma simbólica (`u`, `g`, `o`, `a` con `+`, `-`, `=`) es mejor cuando quieres **cambiar un bit sin alterar los demás**.

### Ejemplo
Preparar las carpetas de Itzamna.Dionisio en una VM, con la app corriendo como el usuario `dionisio`:

```bash
sudo useradd --system --no-create-home --shell /usr/sbin/nologin dionisio
sudo mkdir -p /opt/dionisio /var/dionisio/archivos

 # Binarios: los administra root, la app solo los lee y ejecuta
sudo chown -R root:dionisio /opt/dionisio
sudo chmod -R u=rwX,g=rX,o= /opt/dionisio

 # Datos: la app escribe aquí
sudo chown -R dionisio:dionisio /var/dionisio/archivos
sudo chmod 750 /var/dionisio/archivos

 # Archivo con secretos de configuración: solo la app lo lee
sudo install -o dionisio -g dionisio -m 600 appsettings.Production.json /opt/dionisio/
```

**Por qué `chmod 777` está mal:** da lectura, escritura y ejecución a **cualquier usuario y proceso** del sistema. Cualquier servicio comprometido (o un usuario cualquiera) puede modificar tus binarios, plantar código, leer secretos o borrar datos. Además esconde la causa real (usuario o dueño equivocado) en lugar de corregirla. La solución correcta casi siempre es **ajustar el dueño o el grupo** (`chown`) y dar el permiso mínimo necesario.

### Cuándo usarlo y cuándo no
- Diseña permisos con **mínimo privilegio**: la app solo escribe donde lo necesita; los binarios no son escribibles por ella.
- Para necesidades más finas (dar acceso a un usuario extra sin cambiar el grupo), existen las **ACL** (`setfacl`, `getfacl`).
- En contenedores, los mismos conceptos aplican al usuario de la imagen (las imágenes de .NET 8+ incluyen el usuario no root `app`) y a los volúmenes montados.

### Errores comunes
- **`chmod 777`** o `chmod -R 777` como "arreglo".
- **Olvidar la `x` en los directorios padre**: el archivo tiene `644` pero la carpeta no se puede atravesar.
- **`chmod -R 755`** sobre todo un árbol: vuelve ejecutables archivos de datos. Usa `X` mayúscula o `find -type d`/`-type f`.
- **Archivos creados por `root`** (porque desplegaste con `sudo`) que luego la app no puede modificar.
- **Secretos con `644`**: cualquier usuario del servidor los lee.

### Para profundizar
- [chmod(1), página de manual](https://man7.org/linux/man-pages/man1/chmod.1.html)
- [chown(1), página de manual](https://man7.org/linux/man-pages/man1/chown.1.html)
- [Ejecutar contenedores .NET como usuario no root (blog de .NET)](https://devblogs.microsoft.com/dotnet/securing-containers-with-rootless/)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Convierte `rw-r--r--` a octal.
> `644`.

> [!question]- ¿Qué permisos da `chmod 640 archivo`?
> Dueño lectura y escritura, grupo lectura, otros nada (`rw-r-----`).

> [!question]- Haz ejecutable `deploy.sh` solo para el dueño, sin tocar lo demás.
> `chmod u+x deploy.sh`

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué significa `-rwxr-x---`?
> Archivo regular; dueño lectura, escritura y ejecución; grupo lectura y ejecución; otros nada. En octal: `750`.

> [!question]- ¿Qué hace el permiso de ejecución en un directorio?
> Permite entrar y acceder a sus elementos por nombre (`cd`). Sin él, aunque tengas lectura, no puedes abrir archivos dentro.

> [!question]- Error común: resolver un problema de permisos con `chmod 777`. ¿Por qué está mal?
> Cualquier usuario del sistema puede modificar o reemplazar el archivo. Ajusta dueño (`chown`) y da el mínimo necesario.

