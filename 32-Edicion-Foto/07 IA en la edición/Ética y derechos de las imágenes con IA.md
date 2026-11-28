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
# Ética y derechos de las imágenes con IA

Parte de: [[32-Edicion-Foto/07 IA en la edición/_Índice IA en la edición|IA en la edición]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La IA puede ayudar a editar (reducir ruido, seleccionar un sujeto), modificar contenido (borrar o añadir cosas) o **generar** imágenes completas. Cada nivel plantea preguntas distintas: ¿hay que avisar?, ¿la imagen sigue siendo una fotografía?, ¿quién es el autor?, ¿con qué datos se entrenó el modelo? Este tema te da criterios para decidir y lo que dice la ley en México hasta donde está verificado.

### Cuándo debes avisar que usaste IA
- **Siempre que la IA cambie el contenido** y la imagen pueda tomarse como registro de la realidad: añadir o quitar personas u objetos, cambiar el cielo de un lugar real, generar partes enteras.
- **Siempre en contextos donde la regla lo exige**: periodismo, concursos (muchos prohíben la IA generativa; World Press Photo descalifica imágenes generadas o con relleno generativo), clientes o plataformas que piden etiquetar contenido con IA.
- **No es necesario** avisar por herramientas de IA que solo mejoran la técnica sin alterar el contenido, como reducción de ruido o una máscara de sujeto para aclarar una cara, salvo que el contexto lo pida.
- **Regla práctica**: si el espectador cambiaría su opinión sobre la imagen al saberlo, avísalo, en el pie de foto o en la descripción.

### Qué son las Content Credentials (C2PA)
La **C2PA** (*Coalition for Content Provenance and Authenticity*) es una coalición de empresas de tecnología y medios que publica un estándar abierto para registrar el **origen y el historial** de un archivo. Las **Content Credentials** son la implementación visible de ese estándar: un **manifiesto firmado criptográficamente** incrustado en la imagen, que dice quién la creó, con qué cámara o programa, qué ediciones se hicieron y si intervino IA. Si alguien modifica el archivo sin actualizar el manifiesto, la firma deja de coincidir. Algunas cámaras, programas y plataformas ya las escriben o las leen; puedes inspeccionar un archivo en la página de verificación de Content Credentials (Pendiente de confirmar qué apps de tu flujo las escriben; darktable y la a6700 no las mencionan en lo que revisé).

Limitación: los metadatos pueden quitarse (una captura de pantalla los elimina). Las credenciales prueban historial cuando existen; su ausencia no prueba nada.

### Quién es autor de una imagen generada por IA
- **En México**: en julio de 2025, la Segunda Sala de la Suprema Corte resolvió el **Amparo Directo 6/2025** (un avatar generado con Leonardo AI que se quiso registrar ante Indautor) y determinó que las obras creadas **exclusivamente** por IA no están protegidas por la Ley Federal del Derecho de Autor, porque solo las personas físicas pueden ser autoras. Las obras **con intervención humana significativa y demostrable** (dirigir el proceso, seleccionar, ajustar, imponer una visión estética) sí pueden protegerse; qué cuenta como "significativa" aún está por definirse caso por caso.
- **En otros países** el criterio general es parecido (se exige autoría humana), pero cambia por jurisdicción (Pendiente de confirmar para cada país).
- **Tu fotografía editada con IA** (por ejemplo, un ARW de la a6700 con ruido reducido por IA o un objeto borrado) sigue siendo **tu obra**: la captura y las decisiones son humanas.
- **Términos de servicio**: además de la ley, cada herramienta define qué derechos te da sobre lo generado y si usa tus fotos para entrenar. Léelos.

### Ejemplo
- Publicas una foto de Bacalar donde borraste a un turista con Borrador del iPhone 15 Pro: para tu Instagram personal no es obligatorio avisar, pero si la mandas a un concurso de paisaje con reglas de "sin manipulación", no es elegible.
- Haces un póster combinando tu foto con un cielo generado: es un fotomontaje con IA; indícalo, y sabe que la parte generada no tiene protección por sí sola.

### Errores comunes
1. **Asumir que lo generado es tuyo** y registrarlo como obra propia sin intervención humana.
2. **Usar fotos de otras personas como entrada** para generar imágenes sin su permiso.
3. **Quitar metadatos y credenciales** al exportar sin necesidad.
4. **Confundir "mejora técnica" con "alteración"**: la diferencia importa en concursos.

### Para profundizar
- [C2PA](https://c2pa.org/) y [Content Credentials](https://contentcredentials.org/)
- [Explicación de C2PA y Content Credentials (especificación 2.4)](https://spec.c2pa.org/specifications/specifications/2.4/explainer/Explainer.html)
- [Expansión: SCJN, obras hechas con IA no tienen derecho de autor](https://expansion.mx/tecnologia/2025/07/16/obras-hechas-con-ia-no-tienen-derecho-de-autor-scjn)
- [[Derechos de autor de fotos y videos]].

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cuándo debes avisar que usaste IA?
> Cuando el contenido de la imagen cambió de forma relevante o fue generado, sobre todo si se puede tomar como documento real. Muchas plataformas piden o aplican etiquetas de "hecho con IA".

> [!question]- ¿Qué son las Content Credentials (C2PA)?
> Un estándar para adjuntar a un archivo un historial verificable de cómo se creó y editó, incluido el uso de IA.

> [!question]- ¿Quién es autor de una imagen generada por IA?
> En México los derechos de autor se reconocen a personas físicas; criterios recientes apuntan a que lo creado de forma autónoma por IA no se protege igual que una obra humana (verificar con fuentes oficiales). Revisa también los términos de la herramienta que usaste.

## Práctica

- [[Práctica Edición 10 - Quitar objetos, manual vs IA]]

