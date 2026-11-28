---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Edición de fotografía
tags: [edicion-foto]
contenido: completo
---
# Recorte y enderezado

Parte de: [[32-Edicion-Foto/03 Revelado/_Índice Revelado|Revelado]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Recortar** es eliminar parte de los bordes de la imagen para mejorar la composición, quitar distracciones o adaptar la foto a una **proporción** (relación ancho:alto). **Enderezar** es rotar la imagen unos grados para que el horizonte o las verticales queden rectos. Son ajustes sencillos, pero de los que más cambian la impresión de una foto.

### Cómo funciona
**Enderezar**: el ojo detecta enseguida un horizonte inclinado, y la foto se ve descuidada aunque todo lo demás esté bien. En darktable, el módulo *rotate and perspective* tiene una herramienta: clic derecho y arrastra a lo largo del horizonte, y gira lo necesario. En el módulo **crop** eliges la proporción y el área. En RawTherapee está en *Transformar > Rotar* (con línea guía) y *Recortar*.

**Proporciones comunes**:
- **3:2**: sensor de la a6700 y la mayoría de cámaras (6192 × 4128 px).
- **4:3**: iPhone (4032 × 3024 a 12 MP; 8064 × 6048 a 48 MP).
- **1:1**: cuadrado.
- **4:5**: vertical clásico de Instagram (1080 × 1350).
- **16:9**: pantallas, miniaturas de YouTube y fondos de escritorio.
- **9:16**: historias y Reels.

**Cálculo**: para cambiar de proporción conservando un lado, multiplica ese lado por la fracción de la nueva proporción.
- Conservando el ancho: `alto = ancho × (alto_prop / ancho_prop)`.
- Conservando el alto: `ancho = alto × (ancho_prop / alto_prop)`.

```python
# Comprobación rápida de recortes
def alto_para(ancho, prop_w, prop_h):
    return ancho * prop_h / prop_w

print(alto_para(6048, 4, 5))   # vertical iPhone 3:4 -> 4:5 : 7560.0
print(alto_para(4032, 16, 9))  # horizontal 4032 px -> 16:9 : 2268.0
print(alto_para(4128, 4, 5))   # a6700 vertical (4128 de ancho) -> 4:5 : 5160.0
```

### Ejemplo
Foto vertical de la a6700 (4128 × 6192 px, 3:2) para Instagram 4:5: conservando el ancho de 4128 px, el alto queda en 4128 × 5/4 = **5160 px**; recortas 1032 px de alto. Decide de dónde: si hay cielo vacío arriba, quítalo de ahí.

Proceso en darktable:
1. Primero endereza (rotación) y corrige perspectiva.
2. Luego *crop* con proporción 4:5.
3. Activa las guías de tercios o la proporción áurea para colocar al sujeto (ver [[Regla de tercios y proporción áurea]]).

### Cuánto puedes recortar
Depende del destino. Una foto de 48 MP del iPhone o de 26 MP de la a6700 aguanta recortes fuertes para redes: Instagram solo necesita 1080 px de ancho, así que podrías quedarte con una cuarta parte del ancho y seguir sobrado. Para imprimir grande necesitas que queden suficientes píxeles: un 20 × 30 cm a 300 ppp requiere unos 2362 × 3543 px (ver [[Impresión fotográfica (DPI, perfiles y prueba en pantalla)]]).

### Cuándo usarlo y cuándo no
- **Recorta** para eliminar distracciones en los bordes, reforzar la composición o adaptarte a un formato.
- **No recortes de más** para "hacer zoom" si vas a imprimir: mejor acércate al tomar la foto.
- Mantén la proporción original cuando la foto es parte de una serie que se mostrará junta, salvo que todo el conjunto use el mismo recorte.

### Errores comunes
1. **Recortar antes de enderezar**: al rotar se pierden esquinas y tienes que recortar otra vez.
2. **Cortar en articulaciones** (muñecas, rodillas, tobillos) en retratos.
3. **Enderezar contra un horizonte falso**: en un paisaje con colinas, usa el agua o un elemento vertical como referencia.
4. **Olvidar que Instagram recorta la vista de la cuadrícula**: deja el sujeto centrado si importa la vista de perfil.

### Para profundizar
- [darktable 4.6: rotate and perspective](https://docs.darktable.org/usermanual/4.6/en/module-reference/processing-modules/rotate-perspective/)
- [darktable 5.6: flujo recomendado (recorte y encuadre)](https://docs.darktable.org/usermanual/5.6/en/overview/workflow/process/)
- [[Tamaños para web y redes sociales]].

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Una foto vertical del iPhone mide 6048 × 8064 px (3:4). Quieres 4:5 conservando el ancho. ¿Qué alto queda?
> 6048 × 5/4 = **7560 px** de alto (recortas 504 px).

> [!question]- Una foto de 4032 × 3024 px (4:3) a cuadrado 1:1 conservando el alto. ¿Qué tamaño queda?
> **3024 × 3024 px** (recortas 1008 px de ancho).

> [!question]- Quieres 16:9 de una foto horizontal de 4032 px de ancho. ¿Qué alto?
> 4032 × 9/16 = **2268 px**.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Por qué enderezar es lo primero que revisa un espectador?
> Un horizonte inclinado se nota de inmediato y hace ver la foto descuidada (salvo que sea claramente intencional).

> [!question]- ¿Qué proporciones de recorte son comunes?
> 3:2 (cámaras), 4:3 (iPhone), 1:1 (cuadrado), 4:5 (vertical de Instagram), 16:9 (pantallas).

> [!question]- ¿Cuánto puedes recortar sin perder demasiada calidad?
> Depende del uso: una foto de 48 MP aguanta recortes fuertes para redes; para imprimir grande, cuida que queden píxeles suficientes (ver [[Impresión fotográfica (DPI, perfiles y prueba en pantalla)]]).

## Práctica

- [[Práctica Edición 04 - Revelado completo paso a paso]]

