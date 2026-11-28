---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Edición de fotografía
tags: [edicion-foto]
contenido: completo
---
# Relleno generativo y quitar objetos

Parte de: [[32-Edicion-Foto/07 IA en la edición/_Índice IA en la edición|IA en la edición]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Son herramientas para **eliminar** algo de una foto y rellenar el hueco de forma creíble. El **clonado** clásico copia píxeles que ya existen en la foto. El **relleno generativo** usa un modelo de IA que **inventa** píxeles nuevos a partir del contexto (y a veces de una instrucción de texto): puede reconstruir un fondo que no aparece en ninguna otra parte de la imagen, extender un encuadre o reemplazar un objeto por otro.

### Cómo funciona
- **Clonado o corrección**: tú eliges la fuente; el resultado es predecible y fiel a la foto, pero lento en zonas complejas (ver [[Clonar y parchar]]).
- **Relleno por contenido sin IA generativa**: analiza texturas vecinas y sintetiza un relleno (por ejemplo, el complemento Resynthesizer de GIMP). Funciona bien en texturas regulares.
- **Relleno generativo**: un modelo de difusión recibe la foto con la zona enmascarada y genera algo que "encaja" estadísticamente. Es muy rápido en fondos complicados, pero lo que aparece **no existió**.

### En el iPhone 15 Pro
Con **Apple Intelligence**, la app Fotos tiene la herramienta **Borrador** (en inglés, *Clean Up*). Apple indica que es compatible con el iPhone 15 Pro y 15 Pro Max (desde iOS 18.1), y la función está disponible en español. Uso: abre la foto, *Editar* > *Borrador*; el iPhone sugiere objetos a quitar resaltándolos, o tú tocas, encierras en un círculo o pintas sobre lo que quieres eliminar. La edición se puede revertir desde Fotos. En iOS 27 Apple añadió además herramientas para extender y reencuadrar (Pendiente de confirmar cuáles funcionan en el 15 Pro).

Otras opciones: *Corrector* de Snapseed (gratis, no generativo), *Eliminación generativa* de Lightroom (Premium), y en la PC el módulo de retoque de darktable o la herramienta *Corregir* de GIMP. Photopea tiene funciones con IA, pero con límites de uso (Pendiente de confirmar las condiciones actuales).

### Ejemplo
Foto del iPhone 15 Pro en Bellas Artes con un turista atravesando el encuadre:
1. *Editar > Borrador*, pinta sobre el turista.
2. Amplía al 100 %: revisa la zona reconstruida. Es común ver columnas torcidas, texturas borrosas o patrones repetidos.
3. Si el resultado es malo, deshaz y haz varias pasadas pequeñas, o lleva la foto a la PC y combina clonado manual con el relleno.

Con la a6700, revela el ARW en darktable, exporta TIFF y, si quieres IA, pásalo al iPhone o a una herramienta generativa; luego corrige manualmente lo que no convenza.

### Limitaciones
- **Inventa**: puede generar texto ilegible, manos extrañas, bordes incoherentes o elementos que no estaban.
- **Resolución**: muchas herramientas generan a menor resolución que la foto y la zona queda más suave o con distinto ruido; en archivos de 26 o 48 MP puede notarse al imprimir.
- **Zonas grandes y estructuras** (fachadas, rejas, letreros) suelen fallar.
- **Privacidad y nube**: algunas herramientas procesan en servidores; revisa condiciones antes de subir fotos privadas.
- **Ética y reglas**: en concursos documentales, el relleno generativo descalifica; en periodismo, no se admite (ver [[Ética y derechos de las imágenes con IA]]).

### Cuándo usarlo y cuándo no
- **Úsalo** para limpiar fotos personales, de viaje o creativas; ahorra mucho tiempo.
- **Prefiere clonado manual** cuando necesitas precisión en estructuras, cuando la foto se presenta como documento, o cuando el archivo se imprimirá grande.

### Errores comunes
1. No revisar al 100 %.
2. Borrar sombras y reflejos del objeto pero no el objeto completo (o al revés).
3. Usarlo en una foto que vas a mandar a un concurso con reglas estrictas.
4. Abusar: una calle "vacía" artificial puede verse falsa.

### Para profundizar
- [Apple: Borrador, Extender y Reencuadre en el iPhone](https://support.apple.com/es-mx/guide/iphone/xy0grrlxue5w/ios)
- [Apple: requisitos de Borrador en Fotos](https://support.apple.com/es-lamr/121429)
- [World Press Photo: qué cuenta como manipulación](https://www.worldpressphoto.org/contest/verification-process/what-counts-as-manipulation)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre quitar un objeto con clonado y con relleno generativo?
> El **clonado** copia píxeles que existen en la foto. El **relleno generativo** inventa píxeles nuevos con un modelo de IA, a partir del contexto y a veces de una instrucción en texto.

> [!question]- ¿Qué opción tiene el iPhone 15 Pro?
> La herramienta **Limpiar** de la app Fotos, que forma parte de Apple Intelligence; su disponibilidad depende del idioma, la región y la versión de iOS (Pendiente de confirmar).

> [!question]- ¿Qué limitaciones tiene?
> Puede crear texturas falsas, detalles incoherentes o repetir patrones; revisa al 100 %. Y cambia la naturaleza de la foto: ya no documenta lo que había.

## Práctica

- [[Práctica Edición 10 - Quitar objetos, manual vs IA]]

