---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Fundamentos
tags: [fundamentos]
---
# UTF-8 y codificaciones

Parte de: [[02-Fundamentos/06 Datos y formatos/_Índice Datos y formatos|Datos y formatos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Hay que separar dos ideas:
- **Unicode** es el catálogo universal de caracteres: asigna a cada uno un número llamado **code point**. `A` es U+0041, `ñ` es U+00F1, `€` es U+20AC, `😀` es U+1F600.
- Una **codificación** (encoding) es la regla para convertir esos números en **bytes**. **UTF-8** es la dominante en la web, en Linux, en Git y en JSON. Otras: **UTF-16** (la que usan internamente .NET y JavaScript para sus cadenas), y codificaciones antiguas de un byte como **Latin-1** (ISO-8859-1) y **Windows-1252**.

Todo texto guardado o enviado es bytes más una codificación. Si quien escribe y quien lee no usan la misma, aparece texto corrupto.

### Cómo funciona
**UTF-8** usa de 1 a 4 bytes por code point:

| Rango | Bytes | Ejemplos |
|---|---|---|
| U+0000–U+007F (ASCII) | 1 | `a`, `1`, `{` |
| U+0080–U+07FF | 2 | `ñ` = `C3 B1`, `é` = `C3 A9` |
| U+0800–U+FFFF | 3 | `€` = `E2 82 AC` |
| U+10000–U+10FFFF | 4 | `😀` = `F0 9F 98 80` |

Por eso `año` ocupa **4 bytes** en UTF-8: `a` (1) + `ñ` (2) + `o` (1). Ventajas: compatible con ASCII, sin problemas de orden de bytes y compacto para texto latino.

**UTF-16** usa unidades de 2 bytes; los caracteres fuera del plano básico (como los emojis) necesitan **dos unidades** (un *par subrogado*). Por eso en JavaScript y en C#, `"😀".length` es **2**, aunque sea un solo carácter visible. Si cortas una cadena por longitud, puedes partir un emoji a la mitad.

**Mojibake.** Ves `CafÃ©` en lugar de `Café` cuando se **escribieron bytes UTF-8** (`C3 A9` para `é`) y se **leyeron como Latin-1 o Windows-1252**, que interpreta cada byte como un carácter: `C3` → `Ã` y `A9` → `©`. El arreglo no es reemplazar caracteres, sino declarar y usar **la misma codificación en todo el camino**: archivo, conexión a base de datos, header `Content-Type: text/html; charset=utf-8`, editor.

**BOM** (Byte Order Mark). En UTF-8 es la secuencia `EF BB BF` al inicio de un archivo. No es necesaria (UTF-8 no tiene orden de bytes), pero:
- **Ayuda**: Excel la usa para detectar que un CSV es UTF-8 (ver [[CSV]]).
- **Estorba**: rompe scripts de shell (el `#!` ya no está en el byte 0), algunos parsers JSON estrictos, la concatenación de archivos y comparaciones de cadenas (`﻿` invisible al inicio).

**Finales de línea.** Windows usa **CRLF** (`\r\n`) y Linux/macOS **LF** (`\n`). Mezclarlos produce diffs donde "cambian" todas las líneas y scripts de bash que fallan con `$'\r': command not found` cuando se editaron en Windows. Se controla en Git con `.gitattributes`.

### Ejemplo
Verificaciones rápidas en Python:

```python
print(len("año".encode("utf-8")))       # 4
print("😀".encode("utf-8").hex(" "))      # f0 9f 98 80 → 4 bytes
print(len("😀".encode("utf-16-le")) // 2) # 2 unidades UTF-16
print("Café".encode("utf-8").decode("latin-1"))  # CafÃ© (así se produce el mojibake)
```

En .NET, `File.ReadAllText` asume UTF-8 si no hay BOM; para leer un archivo heredado en Windows-1252 hay que registrar el proveedor de codificaciones:

```csharp
Encoding.RegisterProvider(CodePagesEncodingProvider.Instance);
var texto = File.ReadAllText("clientes_legado.txt", Encoding.GetEncoding(1252));
File.WriteAllText("clientes.txt", texto, new UTF8Encoding(encoderShouldEmitUTF8Identifier: false)); // UTF-8 sin BOM
```

En SQL Server, guarda texto con acentos en columnas `nvarchar` (UTF-16) o en `varchar` con una intercalación `_UTF8` (disponible desde SQL Server 2019); con `varchar` y una intercalación Latin1, los caracteres fuera de esa página se pierden como `?`.

Finales de línea en un repo de Azure DevOps con proyectos .NET y scripts bash, archivo `.gitattributes`:

```text
* text=auto
*.sh text eol=lf
*.ps1 text eol=crlf
*.sln text eol=crlf
*.png binary
```

Para convertir un archivo ya existente de CRLF a LF: `dos2unix script.sh`, o `sed -i 's/\r$//' script.sh`; en VS Code, clic en el indicador `CRLF` de la barra de estado y elegir `LF`.

### Cuándo usarlo y cuándo no
- **UTF-8 sin BOM** por defecto para código, JSON, configuración, scripts y APIs.
- **UTF-8 con BOM** solo cuando el consumidor lo necesita (CSV para Excel).
- **UTF-16** es interno de .NET y JS; rara vez lo eliges para archivos.
- **Codificaciones de un byte** solo al leer o escribir sistemas heredados que las exigen.

### Errores comunes
1. **No declarar la codificación** en archivos, headers o conexiones. Mojibake.
2. **Contar caracteres con `.length`** en JS o C#. Cuenta unidades UTF-16; usa `Array.from(s).length` en JS o `StringInfo` en .NET para caracteres visibles.
3. **Truncar por bytes** una cadena UTF-8. Puedes partir un carácter de 2 a 4 bytes.
4. **BOM en scripts de shell** o en JSON consumido por parsers estrictos.
5. **No configurar finales de línea en Git.** Diffs ruidosos y scripts rotos.

### Para profundizar
- [RFC 3629: UTF-8](https://www.rfc-editor.org/rfc/rfc3629)
- [Microsoft Learn: codificación de caracteres en .NET](https://learn.microsoft.com/dotnet/standard/base-types/character-encoding-introduction)
- [Git: documentación de gitattributes](https://git-scm.com/docs/gitattributes)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- ¿Cuántos bytes ocupa `año` en UTF-8?
> 4: `a` (1) + `ñ` (2) + `o` (1). `len('año'.encode('utf-8'))` → 4.

> [!question]- ¿Cuántos bytes ocupa el emoji 😀 en UTF-8 y cuántas unidades UTF-16 (`.length` en JS)?
> 4 bytes en UTF-8 y **2** unidades UTF-16 (par subrogado), por eso `'😀'.length === 2`.

> [!question]- Convierte un archivo de CRLF a LF con una herramienta de línea de comandos.
> `dos2unix archivo.sh` o `sed -i 's/\r$//' archivo.sh`; en VS Code, el indicador `CRLF` de la barra de estado.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre Unicode y UTF-8?
> **Unicode** asigna un número (code point) a cada carácter (`ñ` = U+00F1). **UTF-8** es una forma de convertir esos números en bytes: 1 byte para ASCII y 2 a 4 para los demás (`ñ` = `C3 B1`).

> [!question]- Ves `CafÃ©` en lugar de `Café`. ¿Qué pasó?
> Se escribieron bytes UTF-8 y se leyeron como Latin-1/Windows-1252 (mojibake). Hay que declarar y leer la misma codificación en todo el camino.

> [!question]- ¿Qué es el BOM y cuándo estorba?
> Byte Order Mark: `EF BB BF` al inicio de un archivo UTF-8. Ayuda a Excel a detectar UTF-8, pero rompe scripts de shell (`#!`), algunos parsers JSON y concatenaciones.

> [!question]- ¿Qué problema causan los finales de línea CRLF vs LF en Git?
> Windows usa `\r\n` y Linux/macOS `\n`. Mezclarlos ensucia diffs y rompe scripts bash con `\r`. Se controla con `.gitattributes` (`* text=auto`, `*.sh text eol=lf`).

