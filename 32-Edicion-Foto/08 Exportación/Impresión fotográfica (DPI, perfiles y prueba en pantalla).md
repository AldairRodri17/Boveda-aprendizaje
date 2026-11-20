---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Edición de fotografía
tags: [edicion-foto]
---
# Impresión fotográfica (DPI, perfiles y prueba en pantalla)

Parte de: [[32-Edicion-Foto/08 Exportación/_Índice Exportación|Exportación]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Imprimir bien una foto implica tres cosas: tener **suficientes píxeles** para el tamaño del papel (resolución, medida en **ppp** o *ppi*, píxeles por pulgada; en impresión se dice coloquialmente DPI), usar el **perfil de color** correcto de la impresora y el papel, y hacer una **prueba en pantalla** (*soft proofing*) para anticipar cómo se verá en papel antes de gastar.

### Cómo funciona: resolución
Fórmula: `píxeles = (centímetros / 2.54) × ppp`, y al revés, `centímetros = píxeles / ppp × 2.54`.

```python
def px(cm, ppp=300):
    return round(cm / 2.54 * ppp)

def cm(pixeles, ppp=300):
    return round(pixeles / ppp * 2.54, 1)

print(px(10), px(15))        # 10 x 15 cm -> 1181 x 1772 px
print(px(20), px(30))        # 20 x 30 cm -> 2362 x 3543 px
print(cm(8064), cm(6048))    # iPhone 48 MP -> 68.3 x 51.2 cm
print(cm(4032), cm(3024))    # iPhone 12 MP -> 34.1 x 25.6 cm
print(cm(6192), cm(4128))    # Sony a6700 26 MP -> 52.4 x 35.0 cm
```

- **300 ppp** es la recomendación para fotos que se ven **de cerca** (álbum, 10 × 15, 20 × 30, cuadro pequeño).
- 200–240 ppp suelen bastar para tamaños medianos; los pósters que se ven a varios metros aguantan 100–150 ppp, porque el ojo no distingue el detalle a esa distancia.
- Si te faltan píxeles, el laboratorio o el software interpolan; un poco no se nota, mucho sí.

### Cómo funciona: perfiles y prueba en pantalla
- Cada combinación **impresora + papel + tinta** tiene un **perfil ICC** que describe qué colores puede reproducir. Su gama suele ser menor que la del monitor y el papel no tiene negros tan profundos ni blancos tan brillantes.
- La **prueba en pantalla** simula en tu monitor (calibrado) el resultado con ese perfil: verás colores que se apagan, sombras que se empastan y brillo reducido. En darktable, en el cuarto oscuro, el botón de **prueba en pantalla** (en la barra inferior) usa el perfil que elijas y puede marcar los colores fuera de gama. En GIMP: `Ver > Gestión del color > Prueba de color`.
- Corrige en una **copia virtual** para impresión: sube un poco la exposición o abre sombras y baja saturación en los colores fuera de gama. Así no arruinas la versión para pantalla.

### Qué le pides al laboratorio
1. Su **perfil ICC** para el papel que elijas (para la prueba en pantalla), y qué **espacio de color** aceptan (muchos piden sRGB; los profesionales aceptan Adobe RGB).
2. **Formato** y tamaño de archivo: JPEG calidad máxima o TIFF, a qué ppp y con qué medidas exactas (incluido el margen o rebase si lo hay).
3. **Tipo de papel**: brillante, lustre/perla o mate (el mate tiene negros menos profundos).
4. Que **no apliquen corrección automática** de color o brillo si ya preparaste el archivo.
5. Una **prueba pequeña** antes de una impresión grande.

### Ejemplo
Quieres imprimir a 30 × 45 cm una foto de la a6700: a 300 ppp necesitas 3543 × 5315 px y tienes 4128 × 6192, así que sobra. Tu recorte 4:5 de 4128 × 5160 a 40 × 50 cm daría unos 262 ppp: suficiente. Una foto de 12 MP del iPhone (4032 × 3024) a 30 × 45 cm quedaría en unos 230 ppp: aceptable; pero a 50 × 75 cm bajaría a unos 137 ppp, así que para ampliaciones grandes usa los 48 MP (o ProRAW 48 MP).

### Cuándo usarlo y cuándo no
- **Prueba en pantalla**: siempre antes de imprimir algo importante, y solo tiene sentido con monitor calibrado.
- **300 ppp**: cuando la foto se verá a menos de un metro. No te obsesiones con 300 en un póster.

### Errores comunes
1. **Monitor demasiado brillante**: la impresión sale oscura (ver [[Calibración del monitor]]).
2. **Confundir el campo "DPI" de los metadatos** con la resolución real: lo que importa son los píxeles.
3. **Enviar Adobe RGB** a un laboratorio que espera sRGB: colores apagados.
4. **No pedir una prueba** antes de un tamaño grande.

### Para profundizar
- [Introducción a los perfiles ICC](https://www.color.org/getting-started/)
- [darktable 5.6: módulo de exportación](https://docs.darktable.org/usermanual/5.6/en/module-reference/utility-modules/shared/export/)
- [Documentación de GIMP 3.0](https://docs.gimp.org/3.0/en/)
- [[Espacios de color (sRGB, Adobe RGB y Display P3)]].

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- ¿Cuántos píxeles necesitas para 10 × 15 cm a 300 ppp? (1 in = 2.54 cm)
> 10 / 2.54 × 300 ≈ **1181 px** y 15 / 2.54 × 300 ≈ **1772 px**.

> [!question]- ¿Y para 20 × 30 cm a 300 ppp?
> ≈ **2362 × 3543 px**.

> [!question]- Una foto de 48 MP del iPhone (8064 × 6048 px), ¿a qué tamaño imprime a 300 ppp?
> 8064 / 300 × 2.54 ≈ 68.3 cm y 6048 / 300 × 2.54 ≈ 51.2 cm → **≈ 68 × 51 cm**.

> [!question]- Una de 12 MP (4032 × 3024 px)?
> ≈ **34 × 26 cm** a 300 ppp.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué resolución se recomienda para imprimir de cerca?
> Alrededor de **300 ppp** (píxeles por pulgada). Para pósters vistos de lejos, menos puede bastar (150–200).

> [!question]- ¿Qué es la prueba en pantalla (soft proofing)?
> Simular en el monitor cómo se verá la foto con el perfil ICC de la impresora y el papel, para corregir antes de imprimir. darktable y GIMP la tienen.

> [!question]- ¿Qué le pides al laboratorio de impresión?
> Su perfil ICC o espacio de color recomendado (a menudo sRGB), tamaño exacto en píxeles y si aplican corrección automática.

## Práctica

- [[Práctica Edición 11 - Exportar para Instagram e impresión]]

