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
# Gaussian splatting en IA 3D

Parte de: [[Representaciones 3D (malla, NeRF, Gaussian splatting y campos implícitos)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Gaussian splatting** (3DGS) representa una escena con miles o millones de **gaussianas 3D**: pequeños elipsoides difusos, cada uno con posición, tamaño, orientación, color y opacidad. Al dibujarlos encimados ("salpicados" en la pantalla) reproducen la apariencia del objeto con mucho realismo y en tiempo real. Lo vas a encontrar en apps de escaneo del iPhone y como una de las salidas de modelos como TRELLIS.

### Cómo funciona
1. **Inicio:** se parte de una nube de puntos (de fotogrametría o de un modelo de IA).
2. **Gaussianas:** cada punto se vuelve un elipsoide con color y transparencia.
3. **Optimización** (en escaneo): se ajustan posición, forma y color para que, vistas desde las cámaras de las fotos, coincidan con las imágenes reales. Se agregan gaussianas donde falta detalle y se quitan donde sobran.
4. **Render:** se proyectan todas a la pantalla, se ordenan por profundidad y se mezclan. No hay rayos por píxel como en NeRF, por eso es muchísimo más rápido.

En modelos generativos como TRELLIS, la red produce una representación latente estructurada y un **decodificador** la convierte en gaussianas para verla, y **otro** decodificador la convierte en malla GLB para exportar.

### Ventaja y desventaja frente a una malla
| | Gaussian splatting | Malla |
|---|---|---|
| Realismo visual | Muy alto: reflejos, pelo, bordes suaves | Depende de texturas y materiales |
| Velocidad de visualización | Tiempo real | Tiempo real |
| Superficie cerrada | **No existe** | Sí (si está bien hecha) |
| Edición en Blender | Limitada, requiere complementos | Total |
| Impresión 3D | **No** directamente | Sí |

La clave: un splat describe **apariencia**, no un sólido. Las gaussianas se traslapan, son semitransparentes y no hay un "dentro" y "fuera" definido. Convertirlo a malla exige un algoritmo extra (o que la herramienta ya exporte una malla), y el resultado suele necesitar limpieza.

### Ejemplo
Escaneas con el iPhone 15 Pro una figura de barro que hizo tu sobrino. La app te ofrece exportar el escaneo como **splat** (`.ply` o `.spz`) y como **malla** (`.obj`, `.glb` o `.usdz`).
- El **splat** lo usas para compartir una vista 3D bonita o verla en el celular.
- La **malla** la llevas a Blender: borras la mesa que se escaneó por debajo, la cierras con Voxel Remesh, le das base plana y la escalas. Ver [[Limpiar un escaneo y hacerlo imprimible]].

Lo mismo con TRELLIS en Hugging Face: el visor muestra gaussianas, pero tú descargas el **GLB**.

### Cuándo usarlo y cuándo no
- **Úsalo** para ver, presentar o documentar un objeto con realismo; para escaneos de escenas grandes (un cuarto, una fachada con el dron).
- **No** lo uses como archivo de impresión; ni siquiera como paso intermedio si la herramienta ya te da una malla.
- Si solo tienes splat, busca en la herramienta una opción de "exportar malla" o "mesh"; convertir por tu cuenta es posible con software adicional, pero la calidad varía (Pendiente de confirmar qué herramienta gratuita da mejor resultado en Windows).

### Errores comunes
1. **Intentar laminar un `.ply` de splats.** El laminador no encuentra caras o interpreta una nube de puntos; descarga la malla.
2. **Juzgar el escaneo por el splat.** El splat oculta huecos con transparencia; la malla del mismo escaneo puede salir con agujeros. Revisa la malla.
3. **Esperar que el color del splat pase a la impresión.** Para imprimir en color con CANVAS pintas tú las zonas en el laminador ([[Separar un modelo por colores para CANVAS]]).

### Para profundizar
- [Implementación oficial de 3D Gaussian Splatting (Inria)](https://github.com/graphdeco-inria/gaussian-splatting)
- [Artículo de 3DGS en Hugging Face Papers](https://huggingface.co/papers/2308.04079)
- [TRELLIS en GitHub](https://github.com/microsoft/TRELLIS)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es Gaussian splatting?
> Una representación con miles o millones de **gaussianas 3D** (elipsoides difusos con color y opacidad) que se renderizan muy rápido. Se usa en escaneo y en modelos como TRELLIS.

> [!question]- ¿Ventaja y desventaja frente a una malla?
> Ventaja: se ve muy realista y se renderiza en tiempo real. Desventaja: no hay superficie cerrada; para imprimir necesitas una malla exportada aparte.

> [!question]- Caso: tu app de escaneo del iPhone exporta splat y malla. ¿Cuál llevas a Blender para imprimir?
> La **malla** (OBJ/GLB/USDZ). El splat solo sirve para verlo o compartirlo.

