---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Drones
tags: [drones]
---
# Despegue, vuelo estacionario y aterrizaje con dron

Parte de: [[Maniobras básicas con dron]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Despegar, mantenerse flotando quieto (vuelo estacionario o *hover*) y aterrizar son la primera de las [[Maniobras básicas con dron]] y las que más se repiten. Son también los momentos de más riesgo: cerca del suelo, de personas y de obstáculos, con la batería recién puesta o casi vacía. Si dominas estos tres pasos, el resto del vuelo se vuelve mucho más tranquilo.

### Cómo funciona
**Despegue seguro**:
1. Superficie **plana, dura y despejada**, o una base de aterrizaje con patrón. Nada de pasto alto, arena suelta ni agua.
2. Checklist completo y **Home Point registrado** (ver [[Revisión antes de despegar (checklist)]]).
3. Despega con el **botón de despegue automático** de DJI Fly (el dron sube y se queda a poca altura) o subiendo suavemente la palanca izquierda.
4. **Quédate flotando a 2–3 m** unos segundos: revisa que no derive, que no vibre y que la imagen esté bien.

**Vuelo estacionario**: con buena señal GNSS y sensor inferior funcionando, el dron se queda casi quieto solo. Tu trabajo es **observar** y corregir con toques pequeños si el viento lo mueve. Practícalo con el dron de espaldas, de lado y de frente.

**Aterrizaje**:
1. Coloca el dron sobre el punto de aterrizaje a unos 2 m.
2. Baja con la palanca izquierda poco a poco (o usa el botón de aterrizaje automático).
3. Cerca del suelo baja despacio; al tocar, mantén la palanca abajo hasta que los motores se detengan.

```mermaid
flowchart LR
  A[Checklist y Home Point] --> B[Despegue]
  B --> C[Hover 2–3 m: revisar]
  C --> D[Vuelo]
  D --> E[Volver sobre el punto]
  E --> F[Bajar despacio]
  F --> G[Motores apagados]
```

**Despegue y aterrizaje con la mano**: hay pilotos que lo hacen, pero con hélices sin protección y un dron sin sensores laterales es arriesgado. Para empezar, usa siempre el suelo o la base.

### Ejemplo
Ejercicio en un campo vacío:

1. Pones una base de aterrizaje y te alejas 5 m.
2. Despegas con el botón automático; el dron queda flotando.
3. Subes a 3 m y lo dejas quieto 30 segundos. Observas la deriva y corriges con toques.
4. Lo giras para que te dé el frente y repites 30 segundos.
5. Bajas y aterrizas sobre la base. Repites cinco veces, y en las últimas intentas aterrizar dentro de un círculo de 50 cm.

Así sabrás que dominas la maniobra cuando la haces **tres veces seguidas** sin correcciones bruscas y aterrizando donde quieres.

### Cuándo usar despegue automático y cuándo manual
- **Automático**: al empezar, con prisa o con nervios; es predecible.
- **Manual**: cuando ya controlas la palanca y quieres despegar más alto o más rápido.
- **No despegues** si la app muestra avisos de brújula, IMU o batería; tampoco con gente a menos de unos metros.

### Errores comunes
- **Aterrizar sobre pasto alto o arena**: las hélices levantan arena o se enredan, y el sensor inferior pierde referencia. Usa una base o una superficie dura y limpia.
- **Despegar antes del Home Point**.
- **Subir de golpe** sin revisar el hover.
- **Bajar rápido cerca del suelo**: rebote o vuelco.
- **Soltar la palanca antes de que se detengan los motores**.

### Para profundizar
- [Manual de usuario del DJI Mini 4K (PDF)](https://dl.djicdn.com/downloads/DJI_Mini_4K/20260427/DJI_Mini_4K_User_Manual_v1.0_zh-cn.pdf)
- [DJI: especificaciones del Mini 4K (sistema de visión)](https://www.dji.com/mini-2-se/specs)
- Laboratorio: [[Lab 2 - Seis maniobras básicas en simulador y en campo abierto]]

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo haces un despegue seguro con el Mini 4K?
> Desde una superficie plana y despejada (o una base de aterrizaje), con el Home Point registrado; despega con el botón de despegue automático o subiendo suavemente la palanca izquierda y quédate flotando a ~2–3 m para revisar que el dron esté estable.

> [!question]- Error común: aterrizar sobre pasto alto o arena. ¿Qué riesgo hay?
> Las hélices levantan arena o se enredan en el pasto; el sensor inferior pierde referencia. Usa una base de aterrizaje o una superficie dura y limpia.

## Práctica

- [[Lab 2 - Seis maniobras básicas en simulador y en campo abierto]]

