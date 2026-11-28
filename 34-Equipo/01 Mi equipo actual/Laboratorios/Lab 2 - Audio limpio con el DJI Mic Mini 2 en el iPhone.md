---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Equipo
herramienta: DJI Mic Mini 2, iPhone 15 Pro, app Blackmagic Camera, app DJI Mimo, DaVinci Resolve (gratuito)
tags: [equipo, laboratorio]
contenido: completo
---
# Lab 2 - Audio limpio con el DJI Mic Mini 2 en el iPhone

Parte de: [[34-Equipo/01 Mi equipo actual/_Índice Mi equipo actual|Mi equipo actual]]

**Cubre:** [[DJI Mic Mini 2]] · [[Blackmagic Camera (app)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Conectar el Mic Mini 2 al iPhone, comparar los niveles de cancelación de ruido y usar la pista de seguridad (modo Ms).

## Pasos

1. Carga transmisor y receptor en el estuche. Conecta el **receptor móvil** al USB-C del iPhone.
2. Abre **Blackmagic Camera**, entra al menú de audio y confirma que la entrada es el micrófono externo; mira los medidores mientras hablas.
3. Coloca el transmisor a ~15–20 cm de la boca. Graba 15 s en interior callado con cancelación **apagada**.
4. Sal a un lugar con ruido (calle, ventilador) y graba la misma frase con cancelación **Off**, **Low** y **High** (interruptor del receptor móvil).
5. Activa el modo **Ms (pista de seguridad, −6 dB)** desde DJI Mimo (o según la [FAQ](https://www.dji.com/mx/mic-mini-2/faq)) y graba una frase que termine en un grito o risa fuerte.
6. Prueba también la vinculación **Bluetooth directa** del transmisor al iPhone y comprueba si tu app de cámara la toma como entrada.
7. Importa todo a DaVinci Resolve, escucha con audífonos y separa las dos pistas del clip Ms.

## Criterios de terminado

- [ ] Los medidores de Blackmagic Camera se mueven con el Mic Mini 2 (no con el micrófono del iPhone).
- [ ] Tengo 3 clips con ruido (Off/Low/High) y anoté cuál suena mejor.
- [ ] En el clip Ms, la pista de respaldo no satura donde la principal sí.
- [ ] Anoté si el Bluetooth directo funciona o no en mi app de cámara.

## Solución

> [!success]- Resultado típico de la cancelación
> **Low** quita ruido de fondo con poca distorsión; **High** limpia más pero deja la voz metálica. Acercar el micrófono mejora más que subir la cancelación.

> [!success]- Cómo usar la pista de seguridad en Resolve
> En la página **Edit**, clic derecho en el clip > *Clip Attributes* > Audio: separa los dos canales en mono. Usa la principal y corta a la de −6 dB solo en el tramo saturado; sube 6 dB su volumen para igualar.

> [!success]- Si el Bluetooth no funciona
> Es esperado: DJI avisa que depende de la app y del sistema. Para video usa el **receptor USB-C**; además, con Bluetooth la cancelación queda fija en *Strong*.

## Notas personales

_Qué me costó, errores que cometí, dudas._
