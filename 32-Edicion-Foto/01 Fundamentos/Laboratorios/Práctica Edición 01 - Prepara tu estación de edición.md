---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Edición de fotografía
herramienta: PC con Windows + darktable/Ansel
tags: [edicion-foto, laboratorio]
---
# Práctica Edición 01 - Prepara tu estación de edición

Parte de: [[32-Edicion-Foto/01 Fundamentos/_Índice Fundamentos|Fundamentos]]

**Cubre:** [[Flujo de edición de fotos]] · [[Calibración del monitor]] · [[Espacios de color (sRGB, Adobe RGB y Display P3)]] · [[Edición no destructiva]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Dejar lista la computadora: monitor ajustado, carpetas, darktable instalado y configurado para exportar en sRGB.

## Pasos

1. Descarga darktable (o Ansel) desde su sitio oficial e instálalo. Ver [[darktable y Ansel]].
2. Si tu monitor tiene modo sRGB, actívalo. Ejecuta `dccw` en Windows (Calibrar color de la pantalla) y sigue el asistente.
3. Crea la estructura: `Fotos/AAAA/AAAA-MM-DD Evento/` y dentro `Exportadas/`.
4. En darktable, importa una carpeta con **añadir a la biblioteca** (sin copiar ni mover archivos).
5. Califica con estrellas (teclas 1–5) y rechaza (R) las fotos malas. Filtra para ver solo 3 estrellas o más.
6. En el módulo de exportación, configura JPEG, perfil **sRGB**, tamaño 2048 px de lado largo y carpeta `$(FILE_FOLDER)/Exportadas`.
7. Edita cualquier ajuste en una foto y comprueba que aparece un archivo `.xmp` junto al original.

## Criterios de terminado

- [ ] darktable abre y muestra mis fotos importadas sin haberlas movido de carpeta.
- [ ] Tengo al menos 10 fotos calificadas y sé filtrar por estrellas.
- [ ] Existe un `.xmp` junto a una foto editada y el original no cambió de fecha de modificación.
- [ ] Una exportación de prueba sale en `Exportadas/` en sRGB a 2048 px.

## Solución

> [!success]- Si no ves el .xmp
> Revisa en preferencias → almacenamiento que la escritura de archivos laterales (sidecar XMP) esté activada.

> [!success]- Variables de la ruta de exportación
> `$(FILE_FOLDER)` es la carpeta del original y `$(FILE_NAME)` el nombre sin extensión. Ejemplo: `$(FILE_FOLDER)/Exportadas/$(FILE_NAME)`.

> [!success]- Por qué sRGB
> Es el espacio que verán bien navegadores y redes. Para imprimir puedes hacer otra exportación según lo que pida el laboratorio.

## Notas personales

_Qué me costó, errores que cometí, dudas._
