---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: IA para modelado 3D
tags: [ia-modelado-3d]
contenido: completo
---
# Licencia Tencent Hunyuan 3D Community

Parte de: [[Licencias de modelos de IA 3D]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **Tencent Hunyuan 3D Community License** es la licencia con la que Tencent publica la familia **Hunyuan3D** (Hunyuan3D-2, Hunyuan3D-2mini y relacionados). Es una licencia **sin regalías** que permite uso personal y comercial, pero con tres tipos de límites: un **territorio** donde aplica, un **umbral de usuarios** para uso comercial a gran escala y una **política de uso aceptable**. Para ti, en México y con un uso de hobby o pequeño negocio, es una licencia muy práctica.

### Cómo funciona
**Territorio.** La licencia define un "territorio" donde se puede usar: **todo el mundo excepto la Unión Europea, Reino Unido y Corea del Sur**. México está dentro. Usar el modelo o sus resultados fuera del territorio no está permitido por la licencia.

**Uso comercial.** Permitido sin pagar. Si tus productos o servicios superan **1 millón de usuarios activos mensuales**, debes pedir una licencia a Tencent.

**Restricciones de uso.**
- No usar el modelo ni sus resultados para **mejorar otros modelos de IA** (salvo derivados de Hunyuan3D).
- Respetar la **política de uso aceptable**: prohíbe, por ejemplo, usos militares, suplantación de identidad o contenido ilegal.
- Identificar el contenido **generado por máquina** cuando lo publicas, según lo que indique la licencia.

**Resultados.** Tencent **no reclama derechos** sobre lo que generas; tú eres responsable de esos resultados y de su uso.

Fuente: el [texto de la licencia](https://huggingface.co/tencent/Hunyuan3D-2/blob/main/LICENSE). Verifica la versión de la licencia de cada modelo (2.0, 2.1 y posteriores) en su propio repositorio.

### Ejemplo: tu caso
| Pregunta | Respuesta para ti |
|---|---|
| ¿Puedo usar Hunyuan3D-2mini en México? | Sí, está en el territorio |
| ¿Puedo vender figuras impresas? | Sí; estás muy lejos de 1 millón de usuarios activos mensuales |
| ¿Puedo usarlo en un Space de Hugging Face? | Sí, la licencia del modelo sigue aplicando igual en la nube |
| ¿Puedo regalar figuras a un amigo en España? | Enviar un objeto físico no es "usar el modelo" allá, pero que él use el modelo en la UE sí está excluido. Casos fronterizos: (Pendiente de confirmar con un abogado) |
| ¿Puedo entrenar mi propio modelo con mallas de Hunyuan3D? | No, salvo que sea un derivado de Hunyuan3D |
| ¿Quién es dueño de la figura? | Tencent no la reclama; los derechos de autor dependen de la ley mexicana |

Para publicar en redes o en una tienda, indica que el modelo fue generado con IA (por ejemplo: "forma generada con Hunyuan3D-2mini y editada a mano en Blender"). Así cumples con la identificación y además explicas tu aporte.

### Cómo se compara
- Frente a **MIT** ([[Licencia MIT en modelos de IA 3D]]): MIT no tiene territorio ni política de uso.
- Frente a **Stability** ([[Licencia Stability AI Community]]): Stability mide ingresos y pide registro; Tencent mide usuarios activos y territorio.
- Paquetes como Hunyuan3D-2GP o WinPortable tienen su propia licencia de código, pero los **pesos** siguen bajo esta licencia ([[Licencias de modelos de IA 3D]]).

### Cuándo te importa
- Antes de vender.
- Si te mudas o trabajas para un cliente en la UE, Reino Unido o Corea del Sur.
- Si quisieras usar los resultados para entrenar otra IA.

### Errores comunes
1. **Asumir que es MIT porque está en GitHub.** Lee el LICENSE.
2. **Ignorar el territorio al trabajar con clientes extranjeros.** Revisa dónde se usará el modelo.
3. **Usar mallas generadas como datos de entrenamiento.** Está restringido.
4. **No identificar el contenido generado.** Indícalo al publicar.

### Para profundizar
- [Licencia Tencent Hunyuan 3D Community](https://huggingface.co/tencent/Hunyuan3D-2/blob/main/LICENSE)
- [Hunyuan3D-2 en GitHub](https://github.com/Tencent/Hunyuan3D-2)
- [Tarjeta de Hunyuan3D-2mini](https://huggingface.co/tencent/Hunyuan3D-2mini)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Dónde se puede usar?
> En el "territorio" que define la [licencia](https://huggingface.co/tencent/Hunyuan3D-2/blob/main/LICENSE): todo el mundo **excepto la Unión Europea, Reino Unido y Corea del Sur**. México no está excluido.

> [!question]- ¿Qué condiciones comerciales y de uso tiene?
> Licencia sin regalías; si tus productos o servicios superan **1 millón de usuarios activos mensuales**, debes pedir licencia a Tencent. No puedes usar el modelo ni sus resultados para mejorar otros modelos de IA (salvo derivados de Hunyuan3D), ni usarlos fuera del territorio. Hay una política de uso aceptable (prohíbe, por ejemplo, usos militares o suplantación) y pide identificar el contenido generado por máquina cuando lo publicas.

> [!question]- ¿Quién es dueño del resultado?
> Tencent no reclama derechos sobre los resultados que generas; tú eres responsable de ellos y de su uso.

