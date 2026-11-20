---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Edición de fotografía
tags: [edicion-foto]
---
# Photopea

Parte de: [[32-Edicion-Foto/02 Programas gratuitos/_Índice Programas gratuitos|Programas gratuitos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Photopea** es un editor de imágenes que funciona **en el navegador**, creado por Ivan Kutskir. Su interfaz imita mucho a Photoshop: capas, máscaras, modos de fusión, capas de ajuste, objetos inteligentes, selección de sujeto, pinceles corrector y clonar. Abre y guarda **PSD** con bastante fidelidad, además de JPEG, PNG, WebP, SVG y DNG, entre otros. La versión base es **gratuita con anuncios**; una suscripción Premium quita los anuncios y añade extras (Pendiente de confirmar el precio vigente).

### Cómo funciona
Entras a photopea.com, arrastras un archivo y editas. La aplicación es JavaScript que se ejecuta en tu equipo; el propio sitio afirma que las imágenes se procesan localmente en el navegador, aunque puedes conectar almacenamiento en la nube si quieres (Pendiente de confirmar los detalles de privacidad en su página actual). No necesitas cuenta para lo básico.

Como imita Photoshop, casi cualquier tutorial de Photoshop sirve: `Ctrl+J` duplica capa, las capas de ajuste (*Curvas*, *Niveles*, *Tono/Saturación*) son no destructivas y las máscaras funcionan igual. Guarda tu trabajo como **PSD** para conservar capas.

### Ejemplo
Estás en la laptop del trabajo o en una PC prestada y necesitas:
- **Quitar el fondo** de una foto de producto tomada con el iPhone para un catálogo: `Seleccionar > Sujeto`, refina el borde con *Refinar borde*, agrega una máscara de capa y exporta PNG con transparencia.
- **Abrir un PSD** que te mandó un diseñador y cambiar un texto.
- **Hacer una miniatura** 1280 × 720 con texto para YouTube.

Para RAW de la a6700 no es la herramienta ideal: abre algunos formatos RAW con un revelado básico, pero darktable o RawTherapee te dan mucho más control. Revela allá y trae a Photopea un JPEG o TIFF.

### Cuándo usarlo y cuándo no
- **Úsalo** cuando no puedes instalar nada, para compartir o editar PSD, para tareas rápidas de capas y para seguir tutoriales de Photoshop.
- **Prefiere GIMP o Krita** para trabajos largos, archivos muy grandes (un TIFF de 16 bits de 26 MP pesa unos 150 MB y el navegador lo resiente) o si te molestan los anuncios.
- **Fotos privadas**: aunque el procesamiento sea local, sigues en una página web con anuncios y scripts de terceros. Para fotos íntimas, de menores o de clientes con contrato de confidencialidad, usa una aplicación instalada. No inicies sesión en servicios en la nube desde equipos ajenos y cierra la pestaña al terminar.

### Errores comunes
1. **Cerrar la pestaña sin guardar**: el navegador no guarda tu trabajo automáticamente como un programa instalado; usa `Archivo > Guardar como PSD` seguido.
2. **Exportar JPEG y perder capas**: el JPEG aplana; conserva el PSD.
3. **Trabajar en 8 bits** con ajustes fuertes: aparecen bandas.
4. **Subir fotos sensibles en una PC pública** y dejarlas en Descargas.

### Para profundizar
- [Photopea](https://www.photopea.com/)
- [Photopea en Wikipedia](https://en.wikipedia.org/wiki/Photopea)
- [[GIMP]] y [[Selecciones y quitar fondos]] en este temario.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es Photopea?
> Un editor de imágenes gratuito que funciona en el navegador, con interfaz parecida a Photoshop; abre y guarda PSD. La versión gratis muestra anuncios.

> [!question]- ¿Cuándo es útil?
> En una computadora sin programas instalados, para abrir PSD que te mandaron o para retoques con capas rápidos.

> [!question]- ¿Qué cuidas al usarlo con fotos privadas?
> Revisa su política de privacidad; en general trabaja en tu navegador, pero si tienes dudas no subas material sensible (Pendiente de confirmar).

## Práctica

- [[Práctica Edición 03 - El mismo archivo en cinco programas]]

