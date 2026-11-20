---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Edición de fotografía
tags: [edicion-foto]
---
# Clonar y parchar

Parte de: [[32-Edicion-Foto/04 Ajustes locales y retoque/_Índice Ajustes locales y retoque|Ajustes locales y retoque]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Son las herramientas para **eliminar** algo pequeño de la foto (polvo del sensor, un granito, un cable, una colilla) reemplazándolo con píxeles de otra zona. **Clonar** copia exactamente los píxeles de una zona fuente sobre la zona destino. **Corregir** o **parchar** (*heal*) también toma una fuente, pero mezcla su textura con el color y la luz de alrededor del destino, de modo que el parche se integra aunque la fuente fuera un poco más clara o de otro tono.

### Cómo funciona
- **Clonar**: útil donde hay bordes definidos o patrones que deben continuar (la línea de un muro, el borde de una ventana). No ajusta tono: si la fuente es más clara, se nota.
- **Corregir/parchar**: ideal para piel, cielos y superficies lisas o con textura uniforme. Puede emborronar si lo usas junto a un borde de alto contraste, porque mezcla los colores vecinos.
- **Relleno por contenido o IA**: genera el relleno automáticamente analizando el entorno (ver [[Relleno generativo y quitar objetos]]).

Herramientas gratuitas:
- **darktable**: módulo **retoque** (*retouch*), con modos *clonar*, *corregir* (heal), *rellenar* y *desenfocar*, más una vista por **ondículas** (wavelets) para retocar solo ciertas escalas de detalle. Las formas son editables y no destructivas.
- **RawTherapee**: *Eliminación de manchas* (spot removal), básica.
- **GIMP**: *Clonar* (`C`), *Corregir* (`H`) y *Clonar en perspectiva*. `Ctrl+clic` define la fuente. Además existe *Resynthesizer* como complemento con "Heal selection" (Pendiente de confirmar compatibilidad con GIMP 3.2).
- **Photopea**: pincel corrector puntual, pincel corrector y parche, como en Photoshop.
- **Snapseed**: *Corrector*. **iPhone 15 Pro**: *Borrador* (Apple Intelligence) en Fotos.

### Ejemplo
Paisaje con la a6700 a f/16 donde aparecen manchas de polvo del sensor en el cielo:
1. En darktable, activa *retoque*, elige la forma círculo y el modo *corregir*.
2. Haz clic en cada mancha; darktable propone una fuente cercana, que puedes mover con `Shift+clic`.
3. Activa *mostrar manchas*… o mejor, sube temporalmente el contraste con una curva fuerte en una instancia extra para que aparezcan todas, retócalas y desactiva esa instancia.
4. Copia las formas de retoque a las demás fotos de la sesión (el polvo está en el mismo lugar del sensor).

En GIMP, para un cable sobre una pared de ladrillos: *Clonar* con alineación *Alineado*, siguiendo la línea de las juntas.

### Cuándo usarlo y cuándo no
- **Úsalo** para limpieza: polvo, manchas, pequeñas distracciones, granitos temporales.
- **No lo uses** para cambiar el contenido de una foto documental o de concurso (ver [[Ética del retoque]]).
- Para objetos grandes o fondos complejos, el relleno generativo puede ser más rápido, pero revisa el resultado.

### Errores comunes
1. **Patrones repetidos visibles** (la misma nube o el mismo ladrillo dos veces): ocurren por tomar siempre la fuente del mismo lugar. Cambia la fuente seguido, usa varias zonas, pinceles suaves, rota o refleja la fuente si el programa lo permite, y alterna clonar y corregir.
2. **Corregir junto a bordes contrastados**: arrastra color y deja manchas; clona primero el borde y luego corrige el interior.
3. **Pincel duro**: deja contornos; usa dureza baja salvo en bordes nítidos.
4. **Retocar en la vista reducida**: revisa al 100 %.

### Para profundizar
- [GIMP 3.0: herramienta Corregir (Heal)](https://docs.gimp.org/3.0/en/gimp-tool-heal.html)
- [darktable 5.6: módulos de procesamiento (retouch)](https://docs.darktable.org/usermanual/5.6/en/module-reference/processing-modules/)
- [Ayuda de Snapseed: Corrector](https://support.google.com/snapseed/answer/6157842?hl=en)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre clonar y corregir (heal)?
> **Clonar** copia píxeles exactos de una zona a otra. **Corregir** copia la textura y la mezcla con el color y la luz del entorno.

> [!question]- ¿Qué herramientas gratuitas lo hacen?
> GIMP (Clonar y Saneado), darktable (módulo **retouch**: clonar, corregir, rellenar, desenfocar), Photopea (pincel corrector) y Snapseed (Corrector).

> [!question]- Error común: patrones repetidos visibles. ¿Cómo lo evitas?
> Cambia a menudo el punto de origen, usa pinceles suaves y no clones siempre del mismo lado.

## Práctica

- [[Práctica Edición 06 - Limpieza y retoque de piel]]

