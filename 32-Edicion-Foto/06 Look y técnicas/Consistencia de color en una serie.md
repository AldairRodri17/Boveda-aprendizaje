---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Edición de fotografía
tags: [edicion-foto]
---
# Consistencia de color en una serie

Parte de: [[32-Edicion-Foto/06 Look y técnicas/_Índice Look y técnicas|Look y técnicas]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **serie** es un conjunto de fotos que se ven juntas: un carrusel de Instagram, un álbum de boda, un reportaje de viaje, un portafolio. **Consistencia de color** significa que el balance de blancos, el brillo, el contraste y el look son coherentes de una foto a otra, para que parezcan parte de la misma historia y no de cámaras o días distintos.

### Por qué importa
Cuando una foto del carrusel está más amarilla o más oscura que las demás, el espectador lo percibe como error y deja de mirar el contenido. La consistencia transmite intención y profesionalismo, y en trabajos para clientes (producto, eventos) es obligatoria: el mismo producto no puede cambiar de color entre fotos. Es especialmente difícil cuando **mezclas cámaras**, por ejemplo la a6700 con el iPhone 15 Pro y el DJI Mini 4K, porque cada una tiene su propia ciencia de color.

### Cómo funciona
1. **Primero, balance de blancos y exposición**. Es lo que más se nota y lo primero que debes revisar al igualar. Si un tono de piel o un elemento neutro (pared blanca, camisa gris, asfalto) cambia de color entre fotos, el resto da igual.
2. **Después, contraste y punto de negro/blanco**.
3. **Por último, el look** (gradación, saturación).

Técnicas en darktable:
- **Copiar y pegar historial**: revela una foto de referencia, `Ctrl+Shift+C` para copiar selectivamente (elige solo color y tono, no recorte ni retoque), selecciona las demás y `Ctrl+Shift+V`.
- **Estilos**: guarda la corrección y el look como estilo y aplícalo a la serie (ver [[Presets y LUTs en fotografía]]).
- **Calibración de color con referencia**: en la pestaña CAT del módulo *calibración de color*, puedes **tomar el color de una zona** de la foto de referencia y aplicar ese objetivo a las demás para emparejar el blanco. También tiene la opción de calibrar con una **carta de color** (ColorChecker) si fotografiaste una en la sesión.
- **Comparar lado a lado**: en la mesa de luz, el modo *culling* o *previsualización* con varias fotos a la vez; en el cuarto oscuro, la **segunda ventana** para tener la referencia a la vista mientras editas otra.
- **Instantáneas** (*snapshots*): toma una de la foto de referencia y compárala en pantalla dividida.

### Ejemplo
Carrusel de 8 fotos de una comida: 5 de la a6700 y 3 del iPhone.
1. Elige la mejor foto de la a6700 como referencia; ajusta calibración de color hasta que el mantel blanco sea neutro y la piel natural.
2. Pega su historial (solo color y tono) a las otras cuatro de la a6700. Ajusta exposición individual.
3. Con las del iPhone (ProRAW revelado en RawTherapee o HEIF), iguala a ojo con la referencia al lado: blanco del mantel, luego piel, luego saturación general. El iPhone suele dar más contraste y saturación en HEIF; bájalos.
4. Verifica todas juntas en miniatura: las desviaciones se ven mejor de lejos.

### Cuándo usarlo y cuándo no
- **Siempre** en series para publicar juntas o entregar a clientes.
- No hace falta forzarla si cada foto se publicará suelta en momentos distintos, aunque un estilo reconocible en tu perfil también ayuda.

### Errores comunes
1. **Igualar el look antes que el balance de blancos**.
2. **Pegar el historial completo** (incluye recorte, retoque, máscaras) a fotos que no lo necesitan.
3. **Ignorar el cambio de luz** a lo largo del día: "consistente" no es "idéntico"; una foto de noche puede ser más azul, pero las del mismo momento deben coincidir.
4. **Editar con el monitor sin calibrar** o con brillo automático.

### Para profundizar
- [darktable 4.0: calibración de color](https://docs.darktable.org/usermanual/4.0/en/module-reference/processing-modules/color-calibration/)
- [darktable 5.6: flujo recomendado](https://docs.darktable.org/usermanual/5.6/en/overview/workflow/process/)
- [[Balance de blancos y temperatura de color]].

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Por qué importa la consistencia en una serie?
> Hace que las fotos se vean como un conjunto (portafolio, perfil de Instagram, álbum) y no como fotos sueltas.

> [!question]- ¿Qué técnica usas en darktable para igualar varias fotos?
> Edita una de referencia, **copia** su historial y **pega** de forma selectiva (exposición y balance aparte) en las demás; luego ajusta cada una. Un estilo propio ayuda.

> [!question]- ¿Qué elemento revisas primero al igualar?
> El **balance de blancos** y los tonos de piel: son lo que más delata diferencias.

## Práctica

- [[Práctica Edición 09 - Recrear un look y aplicarlo a una serie]]

