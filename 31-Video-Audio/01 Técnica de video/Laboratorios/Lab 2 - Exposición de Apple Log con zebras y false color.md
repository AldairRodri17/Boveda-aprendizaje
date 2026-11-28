---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Video y audio
herramienta: iPhone 15 Pro + Blackmagic Camera
tags: [video-audio, laboratorio]
contenido: completo
---
# Lab 2 - Exposición de Apple Log con zebras y false color

Parte de: [[31-Video-Audio/01 Técnica de video/_Índice Técnica de video|Técnica de video]]

**Cubre:** [[Perfiles de color en video (estándar, Log y HLG)]] · [[Herramientas de exposición en video (zebras, waveform y false color)]] · [[Códecs y contenedores de video (H.264, H.265 y ProRes)]] · [[Profundidad de bits y submuestreo de croma]] · [[Filtros ND en video]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Grabar la misma escena de alto contraste en perfil estándar y en Apple Log, exponiendo con herramientas de medición y no a ojo, para luego corregirla en edición.

## Pasos

1. Revisa en [[iPhone 15 Pro para foto y video]] que tengas espacio suficiente (ProRes ocupa varios GB por minuto) o conecta un SSD externo por USB-C.
2. Busca una escena con contraste: una persona junto a una ventana con cielo visible.
3. En [[Blackmagic Camera (app)]] activa **zebras** (p. ej. 95 %) y **false color**.
4. Graba 10 s en códec **HEVC** con perfil estándar, exponiendo para que la cara se vea bien.
5. Cambia a **ProRes** con **Apple Log**. Activa la LUT de monitoreo si está disponible. Expón guiándote por false color (cara en el rango de tono de piel) y zebras (que solo marquen el cielo si es inevitable).
6. Graba 10 s en Log con la misma escena y obturación de 180°.
7. Pasa los dos clips a la PC y compáralos (sin corregir) en un reproductor o en Resolve.

## Criterios de terminado

- [ ] Tienes un clip HEVC estándar y un clip ProRes Apple Log de la misma escena.
- [ ] En el clip Log, la cara no aparece en false color como sobreexpuesta ni subexpuesta.
- [ ] Anotaste el tamaño de cada archivo y la diferencia en MB por segundo.
- [ ] Explicas por qué el clip Log se ve gris sin corregir y qué harías para convertirlo (ver Lab 5 de Edición de video).

## Solución

> [!success]- Resultado esperado
> El clip estándar se ve bien de inmediato, pero el cielo probablemente quedó blanco. El clip Log se ve plano y gris, pero conserva más detalle en la ventana y las sombras. El ProRes es mucho más pesado (varias veces el tamaño del HEVC).

> [!success]- Si no ves la opción de Apple Log
> Apple Log requiere el códec ProRes; verifica que estés en ProRes en Blackmagic Camera o en Ajustes > Cámara > Formatos > ProRes en la app de Apple. Algunas combinaciones de resolución/fps con ProRes requieren grabar a un SSD externo.

## Notas personales

_Qué me costó, errores que cometí, dudas._
