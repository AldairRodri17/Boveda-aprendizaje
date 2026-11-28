---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Edición de fotografía
tags: [edicion-foto]
contenido: completo
---
# RawTherapee

Parte de: [[32-Edicion-Foto/02 Programas gratuitos/_Índice Programas gratuitos|Programas gratuitos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**RawTherapee** es un revelador RAW libre y gratuito (GPLv3) para Windows, macOS y Linux. Su motor procesa en punto flotante de 32 bits y es famoso por la calidad de su demosaico, su reducción de ruido y herramientas técnicas como la resta de *dark frame*, *flat field* y el apilado de *pixel shift*. La versión más reciente es la **5.13**, publicada el 26 de julio de 2026. A diferencia de darktable, **no tiene catálogo ni base de datos**: trabaja directamente sobre carpetas.

### Cómo funciona
- **Explorador de archivos**: navegas tus carpetas, ves miniaturas, calificas y mandas a la cola de procesamiento.
- **Editor**: pestañas con herramientas (Exposición, Detalle, Color, Avanzado, Transformar, RAW, Metadatos). Las herramientas se aplican en un orden interno fijo.
- **Cola de procesamiento**: exporta en lote a JPEG, TIFF o PNG.
- **Perfiles de procesamiento (`.pp3`)**: cada edición se guarda en un **archivo sidecar** junto al RAW, por ejemplo `DSC01234.ARW.pp3`, y opcionalmente en una caché. Un `.pp3` es texto plano; puedes guardar uno como "perfil" y aplicarlo a muchas fotos, y RawTherapee incluye perfiles de inicio como *Auto-Matched Curve* que imitan el JPEG de la cámara.

Tiene dos enfoques de tono: el clásico (exposición, compresión de altas luces, curvas) y herramientas avanzadas como *Local Adjustments* (ajustes locales con muchas sub-herramientas) y *Color Management* con perfiles DCP.

### Ejemplo
Tomaste una serie nocturna con la a6700 a ISO 6400 y quieres el máximo detalle:
1. Abre la carpeta, elige una foto y aplica el perfil *Auto-Matched Curve - ISO High*.
2. En **RAW > Demosaicing** prueba el método dual (dos algoritmos combinados según el detalle).
3. En **Detalle > Reducción de ruido** sube primero la *crominancia* (casi gratis) y la *luminancia* con moderación.
4. Guarda el perfil como `Noche_a6700.pp3` y pégalo a las demás (clic derecho > *Perfil de procesamiento > Pegar*).
5. Mándalas a la cola y exporta TIFF 16 bits si vas a retocar en GIMP.

Para el ProRAW del iPhone 15 Pro, RawTherapee incorporó desde la 5.9 la corrección para los predictores de JPEG sin pérdida que usa Apple, así que es una buena opción cuando darktable no abre el archivo (Pendiente de confirmar con los ProRAW comprimidos en JPEG XL).

### Cuándo usarlo y cuándo no
- **Úsalo** para fotos técnicas exigentes (ISO alto, detalle fino, paisaje), cuando darktable no abre un archivo, o si prefieres trabajar por carpetas sin catálogo.
- **Prefiere darktable** cuando necesites catálogo, etiquetado, máscaras dibujadas cómodas, estilos para series grandes o marca de agua automática.
- Puedes usar ambos: no se estorban, porque cada uno usa su propio sidecar (`.xmp` vs `.pp3`), pero la edición **no** pasa de uno a otro.

### Errores comunes
1. **Perder los `.pp3`** al mover solo los RAW: copia la carpeta completa.
2. **Abrir un perfil "neutral" y pensar que la foto está mal**: el RAW sin curva se ve plano; usa un perfil de inicio o ajusta tú.
3. **Subir la nitidez sin ver al 100 %**: las vistas reducidas engañan; RawTherapee solo aplica algunas herramientas de detalle en la vista 1:1.
4. **Esperar velocidad de darktable**: algunas herramientas son lentas; trabaja con la vista previa reducida y revisa detalles al final.

### Para profundizar
- [Sitio oficial de RawTherapee](https://www.rawtherapee.com/)
- [Sidecar files - Processing profiles (RawPedia)](https://rawpedia.rawtherapee.com/Sidecar_Files_-_Processing_Profiles)
- [Crear perfiles de procesamiento de uso general (RawPedia)](https://rawpedia.rawtherapee.com/Creating_processing_profiles_for_general_use)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es RawTherapee?
> Un revelador RAW libre y gratuito, conocido por su calidad de demosaicado y control fino de nitidez y ruido.

> [!question]- ¿Cómo guarda tus ediciones?
> En un archivo de perfil **.pp3** junto a la foto; el original no se toca.

> [!question]- ¿Cuándo lo usarías en vez de darktable?
> Para comparar resultados de un RAW difícil (ruido, detalle fino) o si su interfaz por pestañas te resulta más clara.

## Práctica

- [[Práctica Edición 03 - El mismo archivo en cinco programas]]

