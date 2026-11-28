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
# Calibración del monitor

Parte de: [[32-Edicion-Foto/01 Fundamentos/_Índice Fundamentos|Fundamentos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Calibrar** el monitor es ajustarlo a valores conocidos (brillo, punto blanco y gamma) y luego **perfilarlo**: medir con un sensor cómo reproduce los colores y guardar esa medición en un **perfil ICC**. El sistema y los programas con gestión de color (darktable, GIMP, Photopea en navegador con gestión de color) usan ese perfil para mostrar los colores correctos. Sin calibrar, editas "a ciegas": lo que ves no es lo que hay en el archivo.

### Cómo funciona
1. **Colorímetro**: un sensor USB (Calibrite, Datacolor Spyder, etc.) se coloca sobre la pantalla.
2. **Software**: el del fabricante, o la opción gratuita **DisplayCAL + ArgyllCMS**. DisplayCAL es la interfaz y ArgyllCMS hace las mediciones (el mantenimiento de DisplayCAL ha pasado por un fork en Python 3; Pendiente de confirmar su estado al día de hoy).
3. **Objetivos típicos para foto**: punto blanco **D65 (6500 K)**, gamma 2.2 o curva sRGB, y brillo de **80–120 cd/m²** (en un cuarto con luz tenue). Los monitores de fábrica suelen venir a 250–350 cd/m², demasiado brillantes.
4. El programa muestra parches de color, el sensor los mide y genera un `.icc` que Windows carga en *Administración del color*.

### Ejemplo
Tu PC con GTX 1660 Ti y un monitor IPS:
- Sin colorímetro: en Windows, `Configuración > Pantalla` desactiva brillo/contraste adaptativo y cualquier modo "vívido" o "juego". En el menú del monitor elige el modo **sRGB** si existe, temperatura **6500 K** o "Cálido/Normal" y baja el brillo hasta que una hoja blanca de papel junto al monitor y el blanco de la pantalla se parezcan. Revisa en un patrón de grises que veas separados los tonos del 0 al 5 % y del 95 al 100 %.
- Con colorímetro: calibras a D65, gamma 2.2, 100 cd/m²; recalibras cada uno o dos meses.
- En darktable, comprueba en *preferencias* que el perfil de pantalla sea el del sistema.

El iPhone 15 Pro viene bien calibrado de fábrica en P3, pero desactiva True Tone y el brillo automático mientras editas para juzgar color y brillo.

### Por qué las impresiones salen oscuras
Es el problema número uno: el monitor está **demasiado brillante** y además emite luz, mientras el papel solo la refleja. Si editas a 300 cd/m², tus fotos te parecen bien en pantalla pero en realidad están subexpuestas; al imprimir salen oscuras y empastadas. Bajar el brillo a ~100 cd/m² y usar **prueba en pantalla** (ver [[Impresión fotográfica (DPI, perfiles y prueba en pantalla)]]) lo corrige.

### Cuándo usarlo y cuándo no
- **Imprescindible** si imprimes, vendes fotos o entregas trabajo a clientes.
- **Recomendable** si publicas mucho; aun sin colorímetro, los ajustes manuales evitan los errores más grandes.
- Si tu monitor cubre poco sRGB (algunas laptops económicas), la calibración mejora la precisión pero no amplía la gama.

### Errores comunes
1. **Editar con brillo máximo** o en un cuarto muy oscuro: fotos oscuras al imprimir y con sombras demasiado abiertas en otras pantallas.
2. **Dejar activos modos "vívido", HDR del sistema o brillo automático**: cambian el color mientras editas.
3. **Tener dos perfiles en conflicto** (software del monitor + perfil de Windows). Usa uno.
4. **Calibrar una vez y olvidarse**: los monitores derivan con el tiempo.
5. **Juzgar color con fondo de escritorio saturado**: usa un tema gris neutro en darktable (`darktable-elegant-grey`).

### Para profundizar
- [DisplayCAL](https://displaycal.net/)
- [ArgyllCMS](https://www.argyllcms.com/)
- [Introducción a los perfiles ICC](https://www.color.org/getting-started/)
- [[Espacios de color (sRGB, Adobe RGB y Display P3)]]

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es calibrar el monitor?
> Ajustar y medir la pantalla con un colorímetro para que muestre brillo, blanco y colores estándar, y crear un perfil ICC.

> [!question]- ¿Qué puedes hacer sin colorímetro?
> Usar el modo sRGB del monitor si lo tiene, bajar el brillo para editar, usar la herramienta de Windows "Calibrar color de la pantalla" (`dccw`) para gamma y balance básicos, y revisar tus fotos en el celular y otra pantalla.

> [!question]- ¿Por qué tus impresiones salen más oscuras que en pantalla?
> Casi siempre porque el monitor está demasiado brillante. Para editar fotos a imprimir se suele bajar el brillo a un nivel moderado (alrededor de 100–120 cd/m² con colorímetro).

## Práctica

- [[Práctica Edición 01 - Prepara tu estación de edición]]

