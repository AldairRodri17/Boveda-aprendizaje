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
# Blanco y negro

Parte de: [[32-Edicion-Foto/06 Look y técnicas/_Índice Look y técnicas|Look y técnicas]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Convertir a **blanco y negro** es eliminar el color para que la foto se lea solo por **luz, forma, textura y contraste**. Bien hecho no es una foto "sin color", sino una interpretación nueva: tú decides qué tan claro u oscuro se vuelve cada color original. Sigue siendo uno de los lenguajes más fuertes de la fotografía.

### Qué fotos funcionan mejor
- Con **contraste de luz** marcado: luz dura, contraluces, sombras con formas.
- Con **texturas y formas**: arquitectura, piedra, arrugas, nubes.
- **Retratos** donde importa la expresión más que la ropa.
- Escenas con **colores que distraen** o que no combinan (letreros, luces mezcladas), que en B/N se unifican.
- Fotos de calle y documentales con ambiente atemporal.

Funcionan peor las fotos cuyo interés es justamente el color (atardecer naranja, mercado de flores) o con luz muy plana sin separación tonal.

### Cómo funciona
**Por qué no basta con quitar la saturación**: al desaturar, cada color se convierte en el gris de su luminosidad, y colores muy distintos pueden quedar iguales. Un vestido rojo y un follaje verde, que en color contrastan mucho, pueden volverse el mismo gris medio y la foto queda plana. Una buena conversión **mezcla los canales** a tu elección: como en película se usaban filtros de color, un filtro rojo oscurece cielos azules y aclara la piel; uno verde aclara follaje y oscurece labios; uno azul hace lo contrario que el rojo.

**En darktable**:
- **Calibración de color, pestaña gris**: es el método recomendado por la documentación de 5.6. Mezclas cuánto aporta el rojo, el verde y el azul a la luminosidad, con presets que simulan películas o filtros.
- Alternativas: módulo **monochrome** (filtro de color virtual sobre un gráfico), o *balance de color rgb* con saturación a 0 (equivalente a desaturar).
- Antes de convertir, el ecualizador de color puede ajustar la luminosidad de gamas concretas.
- Después, contraste con el mapeo de tonos o una curva, y **dodge and burn** (la tradición del cuarto oscuro).

### Ejemplo
Retrato en la calle con la a6700, cielo azul de fondo y cara bien iluminada:
1. Revela en color: exposición y blancos/negros.
2. *Calibración de color > gris*: sube rojo y baja azul (efecto filtro rojo): el cielo se oscurece, la piel queda limpia y luminosa.
3. Ecualizador de tonos: un poco más de contraste en medios tonos.
4. Dodge en ojos, burn en bordes.
5. Grano fino para textura.

### Cuándo usarlo y cuándo no
- **Úsalo** con intención: cuando el color no aporta o estorba, para una serie coherente con luz difícil o por estilo.
- **No como rescate automático** de fotos malas: una foto desenfocada o sin interés lo sigue siendo en B/N.

### Errores comunes
1. **Gris plano sin negros ni blancos**: la foto se ve "lavada". Se arregla dando contraste: fija un punto negro real (alguna zona casi negra) y un blanco con algo de detalle, usa una curva en S o el ecualizador de tonos, y separa zonas con la mezcla de canales y dodge and burn. Revisa el histograma: debe ir de lado a lado.
2. **Solo desaturar**.
3. **Exagerar el filtro rojo**: cielos negros con halos y ruido en el borde.
4. **Dominantes involuntarias**: si el B/N tiene un leve tinte, que sea elegido (tono sepia, selenio), no un residuo.

### Para profundizar
- [darktable 5.6: revelar imágenes monocromáticas](https://docs.darktable.org/usermanual/5.6/en/guides-tutorials/monochrome/)
- [darktable 4.0: calibración de color](https://docs.darktable.org/usermanual/4.0/en/module-reference/processing-modules/color-calibration/)
- [[Dodge and burn]] y [[Curvas de tonos]].

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué fotos funcionan mejor en blanco y negro?
> Las de contraste fuerte, textura, formas y luz dramática; también cuando el color distrae.

> [!question]- ¿Por qué no basta con quitar la saturación?
> Porque pierdes control: un buen blanco y negro decide qué colores se vuelven claros u oscuros (p. ej. oscurecer el cielo azul, aclarar la piel).

> [!question]- ¿Cómo lo haces en darktable?
> Con el módulo **calibración de color** (pestaña gris) o **monocromo**, ajustando el peso de cada canal; luego contraste con curvas o ecualizador de tonos.

> [!question]- Error común: gris plano sin negros ni blancos. ¿Cómo se arregla?
> Ajusta los puntos de blanco y negro y agrega contraste: casi todo buen B/N tiene algo de blanco y algo de negro.

## Práctica

- [[Práctica Edición 08 - Blanco y negro con intención]]

