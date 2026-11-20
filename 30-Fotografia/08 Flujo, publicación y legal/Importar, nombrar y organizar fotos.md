---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Fotografía
tags: [fotografia]
---
# Importar, nombrar y organizar fotos

Parte de: [[30-Fotografia/08 Flujo, publicación y legal/_Índice Flujo, publicación y legal|Flujo, publicación y legal]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Importar** es pasar las fotos de la tarjeta o el teléfono a tu computadora; **nombrar y organizar** es darles una estructura de carpetas, nombres y etiquetas que te permita **encontrar cualquier foto en segundos** dentro de cinco años. Es la base de todo el flujo: sin orden, el respaldo, la edición y el portafolio se vuelven un caos.

### Cómo funciona
**Estructura de carpetas sencilla para principiantes:** por **año** y dentro por **fecha-evento**, con la fecha en formato año-mes-día para que se ordene sola:

```text
Fotos/
  2026/
    2026-11-09 Centro Histórico/
      RAW/
      Editadas/
    2026-11-15 Cumpleaños Ana/
  2027/
```

El formato `AAAA-MM-DD` ordena cronológicamente en cualquier sistema operativo. Un nombre corto del evento basta para buscar.

**Nombres de archivo:** las cámaras repiten nombres (`DSC00001.ARW` vuelve a empezar). Renombra al importar con fecha y hora más un contador: `2026-11-09_1534_0001.ARW`. darktable permite patrones de nombre al importar; también se puede con ExifTool usando la fecha de captura.

**Pasar fotos del iPhone a una PC con Windows sin perder calidad:**
1. **Con cable** y la app **Fotos de Windows → Importar**: copia los archivos originales (HEIF, ProRAW DNG, video HEVC/ProRes).
2. **iCloud para Windows**: sincroniza la fototeca; descarga los originales (no solo las versiones optimizadas).
3. En el iPhone, revisa **Ajustes → Fotos → Transferir a Mac o PC**: con **Automático** el iPhone puede convertir a JPEG/H.264 compatibles; con **Mantener originales** recibes los archivos tal cual (mejor calidad; necesitas soporte HEIF/HEVC en Windows).

**Borrar pronto las fotos malas** (movidas, desenfocadas, duplicadas, ráfagas casi iguales) reduce espacio y tiempo de respaldo, y hace más fácil encontrar las buenas. Hazlo en la primera revisión, con calificación por estrellas: rechaza (X), marca 1 estrella las aceptables y 3+ las mejores.

```mermaid
flowchart LR
    T[Tarjeta / iPhone] --> I[Importar a carpeta<br/>AAAA-MM-DD Evento] --> R[Renombrar] --> B[Respaldo inmediato] --> S[Selección: rechazar,<br/>estrellas, etiquetas] --> E[Editar las mejores] --> X[Exportar a Editadas]
```

### Ejemplo
Flujo con tu equipo después de una salida:
1. **Sony a6700**: saca la tarjeta SD y cópiala con el lector a `Fotos/2026/2026-11-09 Centro Histórico/RAW`. No edites directamente desde la tarjeta.
2. **iPhone 15 Pro**: conecta por cable, importa con Fotos de Windows a la misma carpeta (subcarpeta `iPhone`).
3. **Osmo Pocket 4P y DJI Mini 4K**: copia sus tarjetas microSD a subcarpetas `Pocket` y `Dron`.
4. Respaldo inmediato al SSD externo ([[Respaldo 3-2-1 de fotos y videos]]).
5. Abre la carpeta en darktable (vista mesa de luz) o en digiKam (gestor gratuito), rechaza las malas y agrega palabras clave: lugar, personas, tema.
6. Solo después **formatea** las tarjetas en la cámara.

Si te gusta automatizar, un script de PowerShell puede crear la carpeta con la fecha del día y copiar la tarjeta ahí; o ExifTool puede mover archivos a carpetas según su fecha de captura:

```bash
# Mueve fotos a carpetas por fecha de captura (AAAA/AAAA-MM-DD) y las renombra con fecha y hora
exiftool -r "-FileName<DateTimeOriginal" -d "D:/Fotos/%Y/%Y-%m-%d/%Y-%m-%d_%H%M%S%%-c.%%e" E:/DCIM
```

(Prueba primero con una copia; las opciones de ExifTool son potentes y conviene leer su documentación.)

### Cuándo usarlo y cuándo no
Aplica este orden desde la primera foto; reorganizar 20,000 archivos después es mucho peor. Si solo usas el iPhone y la fototeca de iCloud, igual conviene exportar a carpetas por año para tener un respaldo independiente. No pierdas tiempo en esquemas complejos de 10 niveles: año + fecha-evento + palabras clave alcanza para la mayoría.

### Errores comunes
- **Carpetas "Nueva carpeta (3)"** o "Fotos varias": imposibles de buscar.
- **Fechas en formato DD-MM-AAAA**: se ordenan mal.
- **Formatear la tarjeta antes de respaldar**.
- **Importar en "Automático" desde el iPhone** y perder el original HEIF/ProRAW sin darte cuenta.
- **No borrar nunca**: el archivo crece y el respaldo se vuelve lento y caro.

### Para profundizar
- [Apple: transferir fotos y videos del iPhone a una Mac o PC](https://support.apple.com/es-lamr/120267).
- [ExifTool (documentación oficial)](https://exiftool.org/).
- [[Metadatos EXIF e IPTC]] y [[Flujo de edición de fotos]].
- Laboratorio: [[Práctica Foto 11 - Importar, organizar y respaldar]].

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué estructura de carpetas sencilla funciona para principiantes?
> Por año y fecha-evento: `Fotos/2026/2026-11-09 Centro Histórico`. Fácil de ordenar y buscar.

> [!question]- ¿Cómo pasas fotos del iPhone a una PC con Windows sin perder calidad?
> Con cable y la app Fotos de Windows (Importar), o desde iCloud para Windows. Si quieres el original HEIF/ProRAW, revisa Ajustes → Fotos → Transferir a Mac o PC → **Mantener originales**.

> [!question]- ¿Por qué borrar las fotos malas pronto?
> Reduce espacio y tiempo de respaldo, y hace más fácil encontrar las buenas. Elimina las movidas, desenfocadas y duplicadas.

## Práctica

- [[Práctica Foto 11 - Importar, organizar y respaldar]]

