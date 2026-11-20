---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Edición de fotografía
tags: [edicion-foto]
---
# GIMP

Parte de: [[32-Edicion-Foto/02 Programas gratuitos/_Índice Programas gratuitos|Programas gratuitos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**GIMP** (GNU Image Manipulation Program) es un editor de imágenes de píxeles y capas, libre y gratuito, para Windows, macOS y Linux. Es el equivalente gratuito más cercano a Photoshop para **retoque**: capas, máscaras, selecciones, clonado, corrección, texto y filtros. La serie actual es **GIMP 3.2** (marzo de 2026; 3.2.2 a finales de ese mes), que sigue a la 3.0 de 2025 con filtros no destructivos, capas vectoriales y *link layers*.

### Cómo funciona
- **Capas y máscaras**: cada elemento en su capa; las máscaras ocultan o muestran sin borrar.
- **Filtros no destructivos** (desde 3.0): un filtro GEGL queda como efecto editable de la capa. En 3.2 se pueden aplicar a canales y a grupos; un grupo vacío en modo *Pass through* con un filtro afecta a todo lo de abajo, como una capa de ajuste.
- **Herramientas de retoque**: *Clonar*, *Corregir* (heal), *Clonar en perspectiva*, *Difuminar/Enfocar*, *Blanquear/Quemar* (dodge/burn).
- **Formato nativo XCF**: guarda capas, máscaras, rutas y filtros no destructivos. Para compartir con alguien que usa Photoshop, exporta PSD (en 3.2 también PSB para documentos grandes).
- **RAW**: GIMP **no revela RAW por sí mismo**. Al abrir un ARW o un DNG, llama al revelador que tengas instalado: **darktable** o **RawTherapee** (y en 3.2 también **ART**). Se abre ese programa, reveas y al cerrarlo la imagen pasa a GIMP. Si no tienes ninguno, no podrás abrirlo.

### Ejemplo
Retrato de la a6700 ya revelado en darktable:
1. Exporta desde darktable un **TIFF de 16 bits**.
2. En GIMP: `Archivo > Abrir`, duplica la capa (`Shift+Ctrl+D`) y nómbrala "limpieza".
3. Con *Corregir* (`H`), `Ctrl+clic` en piel limpia para la fuente y pinta sobre granitos.
4. Crea una capa nueva en modo **Luz suave** rellena de gris 50 % para dodge and burn (ver [[Dodge and burn]]).
5. Guarda `retrato.xcf` y exporta `retrato_ig.jpg` en sRGB a 1080 × 1350.

**Krita** (gratuita, 5.3/6.0 publicada en marzo de 2026) es la alternativa si te sientes más cómodo pintando con tableta; para foto, GIMP tiene más herramientas de retoque fotográfico.

### Cuándo usarlo y cuándo no
- **Úsalo** para retoque de piel, clonado complejo, fotomontaje, quitar fondos, texto sobre imagen y composiciones con capas.
- **No lo uses** para revelar: exposición, balance de blancos y color hazlos en el revelador con el RAW. Para ajustes globales simples, el revelador es más rápido.
- Si estás en otra PC sin instalar nada, [[Photopea]] cubre lo básico en el navegador.

### Errores comunes
1. **Guardar con "Sobrescribir"** el TIFF o JPEG original: usa *Exportar como* con otro nombre.
2. **Trabajar en 8 bits** cuando vas a hacer curvas fuertes: en `Imagen > Precisión` usa 16 bits o punto flotante para evitar bandas.
3. **Borrar con la goma** en vez de usar máscara de capa.
4. **No asignar el perfil de color**: si GIMP pregunta al abrir, conserva el perfil incrustado.

### Para profundizar
- [Documentación de GIMP 3.0](https://docs.gimp.org/3.0/en/)
- [Notas de la versión de GIMP 3.2](https://www.gimp.org/release-notes/gimp-3.2.html)
- [Herramienta Corregir (Heal) en GIMP 3.0](https://docs.gimp.org/3.0/en/gimp-tool-heal.html)
- [Krita 5.3.0](https://krita.org/en/posts/2026/krita-5.3.0-released/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Para qué usas GIMP en tu flujo?
> Para retoque y composición con capas: clonar, retoque de piel, quitar fondos, fotomontajes y texto. El revelado RAW déjalo a darktable o RawTherapee.

> [!question]- ¿Qué pasa si abres un RAW en GIMP?
> GIMP lo manda a un revelador instalado (darktable o RawTherapee) y recibe la imagen de vuelta.

> [!question]- ¿Qué formato usas para guardar tu trabajo con capas?
> **.xcf** (formato nativo). Exporta aparte a JPEG o PNG para compartir.

## Práctica

- [[Práctica Edición 03 - El mismo archivo en cinco programas]]

