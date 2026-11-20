---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Edición de fotografía
tags: [edicion-foto]
---
# Reducción de ruido con IA

Parte de: [[32-Edicion-Foto/07 IA en la edición/_Índice IA en la edición|IA en la edición]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **reducción de ruido con IA** usa una red neuronal entrenada con miles de pares de fotos (con ruido y limpias) para quitar el ruido de una imagen. A diferencia de los métodos clásicos, que aplican reglas matemáticas fijas (promediar píxeles parecidos, suavizar en ciertas frecuencias), el modelo "aprendió" cómo se ven las texturas reales y trata de reconstruirlas. Los mejores resultados se obtienen sobre el **RAW antes del demosaico**, porque ahí el ruido aún tiene su forma original.

### Cómo funciona
- **Clásica** (denoise profiled de darktable, la reducción de RawTherapee): estima cuánto ruido hay según la cámara y el ISO, y suaviza. Al subirla, borra tanto el ruido como el detalle fino; tienes que elegir entre limpieza y textura.
- **Con IA**: el modelo separa mejor ruido de detalle, así que a ISO alto conserva más textura y bordes limpios. Algunas herramientas además hacen el demosaico en el mismo paso.

**Riesgo principal**: el modelo **puede inventar detalle**. Donde la información se perdió en el ruido, la red rellena con lo que le parece probable: texturas de piel o pelo que no eran así, letras deformadas, patrones en tela o en follaje, o un aspecto "pintado" o demasiado liso. También puede dejar artefactos en zonas que no se parecen a sus datos de entrenamiento, como cielos nocturnos con estrellas (las confunde con ruido). En fotos documentales o técnicas (astro, ciencia) esto importa.

### Opciones gratuitas
- **darktable 5.6**: el subsistema de IA es **opcional, local y gratuito**, apagado por defecto. Al activarlo en preferencias descargas modelos; el módulo **neural restore** ofrece reducción de ruido para RAW y para imágenes ya reveladas (modelos NIND/RawNIND) y escalado. La inferencia funciona en CPU y, según las notas de la versión, la mayoría de los usuarios de Windows ya tienen aceleración por GPU incluida (Pendiente de confirmar el rendimiento con tu GTX 1660 Ti de 6 GB). Un foro de darktable reporta que neural restore no funciona con ciertos RAW; prueba con tus ARW.
- **El propio iPhone 15 Pro** aplica reducción de ruido computacional al disparar HEIF.
- Opciones de pago (Lightroom *Denoise*, DxO PureRAW y otras) existen, pero no son necesarias para empezar.

### Ejemplo
Concierto en un foro con la a6700 a ISO 12800:
1. En darktable, duplica la imagen (`Ctrl+D`) para comparar.
2. Versión A: *denoise (profiled)* clásico.
3. Versión B: *neural restore* en modo raw denoise.
4. Compara al 100 % en la cara, el pelo y un letrero. Si la versión B conserva poros y bordes sin inventar texto raro, quédatela. Si alguna zona se ve pintada, baja la intensidad o mezcla con una máscara.
5. Después aplica nitidez suave y, si hace falta, grano fino.

### Cuándo usarlo y cuándo no
- **Úsala** en ISO alto (eventos, conciertos, interiores, fauna al amanecer) y cuando vas a recortar mucho.
- **No** en astrofotografía sin revisar las estrellas, en fotos que deben ser evidencia exacta, ni cuando el ruido es bajo (a ISO 100–800 la a6700 casi no lo necesita).
- **Concursos**: World Press Photo permite reducción de ruido con IA dentro de límites, pero prohíbe el escalado con IA; revisa siempre las reglas.

### Errores comunes
1. **Aplicarla al 100 % sin comparar**.
2. **Encadenar** reducción de ruido IA con la clásica fuerte: piel de cera.
3. **No revisar texto y patrones** finos.
4. **Esperar velocidad**: en CPU puede tardar mucho por foto; procesa solo las seleccionadas.

### Para profundizar
- [Novedades de darktable 5.6 (IA opcional, neural restore)](https://ubuntuhandbook.org/index.php/2026/06/darktable-released-5-6-0-with-optional-ai-support-heif-export/)
- [darktable 4.6: denoise (profiled)](https://docs.darktable.org/usermanual/4.6/en/module-reference/processing-modules/denoise-profiled/)
- [Foro pixls.us: neural restore con ciertos RAW](https://discuss.pixls.us/t/neural-restore-does-not-work-with-certain-raw-images/58889)
- [[Nitidez y reducción de ruido]].

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿En qué se diferencia la reducción de ruido con IA de la clásica?
> La clásica promedia o filtra píxeles; la de IA usa un modelo entrenado que distingue mejor ruido de detalle, y suele conservar más textura a ISO alto.

> [!question]- ¿Qué riesgo tiene?
> Puede "inventar" detalle (texturas que no existían) o dejar superficies demasiado limpias. Compara siempre con el original.

> [!question]- ¿Hay opciones gratuitas?
> Las opciones gratuitas con IA cambian rápido (Pendiente de confirmar). Mientras tanto, la reducción por perfiles de darktable (*denoise (profiled)*) y exponer bien en la toma dan muy buen resultado.

## Práctica

- [[Práctica Edición 10 - Quitar objetos, manual vs IA]]

