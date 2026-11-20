---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Fotografía
tags: [fotografia]
---
# Fotografía computacional en celulares

Parte de: [[30-Fotografia/07 Avanzado/_Índice Avanzado|Avanzado]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **fotografía computacional** es el uso de **algoritmos** para combinar **varias tomas** o datos adicionales (profundidad, movimiento, reconocimiento de escenas) y producir una imagen mejor de la que daría **una sola exposición** del sensor. Es la razón por la que un celular con sensor pequeño compite con cámaras dedicadas en muchas situaciones: no gana por óptica, gana por procesamiento.

### Cómo funciona
Cuando tocas el disparador del **iPhone 15 Pro**, la cámara ya venía capturando cuadros en un búfer. El procesador elige y fusiona varios con distintas exposiciones, alinea, reduce ruido, ajusta tonos por zonas (cielo, piel, follaje) y aplica nitidez.

Funciones computacionales principales del iPhone 15 Pro (ver [[iPhone 15 Pro para foto y video]]):
1. **Smart HDR**: fusiona exposiciones para conservar cielo y sombras a la vez.
2. **Deep Fusion / Photonic Engine**: en luz media o baja, combina cuadros píxel por píxel para más detalle y menos ruido.
3. **Modo Noche**: muchas tomas durante varios segundos, alineadas y promediadas.
4. **Modo Retrato**: usa información de profundidad para simular desenfoque de fondo; en el 15 Pro puedes ajustar el desenfoque y el punto de enfoque después.
5. **Fotos de 24 MP** por defecto, que combinan datos del sensor de 48 MP; y la opción de 48 MP en HEIF o ProRAW.
6. **Live Photos** (y su efecto de Exposición larga), **Pano** y **Time-lapse**.

```mermaid
flowchart LR
    B[Búfer de cuadros<br/>antes y después del disparo] --> A[Alineación] --> F[Fusión de exposiciones] --> S[Segmentación:<br/>cielo, piel, fondo] --> T[Tono local,<br/>ruido y nitidez] --> O[HEIF / JPEG / ProRAW]
```

**Límites:** el procesamiento a veces da un **look "sobreprocesado"**: sombras demasiado abiertas (escenas nocturnas que parecen de día), nitidez excesiva, piel suavizada, cielos con halos o colores muy uniformes. Además, el procesamiento es igual para todos, así que muchas fotos de celular se parecen entre sí. **Cómo controlarlo:**
- **ProRAW**: conserva mucha información y se revela a tu gusto (aunque ya incluye parte del procesamiento computacional).
- **Estilos fotográficos**: ajustan tono y calidez de forma consistente desde la toma.
- **Apps manuales** que dan más control (por ejemplo, para video, [[Blackmagic Camera (app)]]).
- Desactivar el modo Noche cuando quieres la atmósfera oscura real.

**Por qué se ve distinto en la PC:** el iPhone guarda muchas fotos con **HDR** (información de brillo extra) y en espacio de color **Display P3**, más amplio que sRGB. Su pantalla muestra ese brillo y color; un monitor o programa sin soporte de HDR o de gestión de color lo muestra más apagado o con colores distintos. Ver [[Espacios de color (sRGB, Adobe RGB y Display P3)]].

### Ejemplo
Experimento con el iPhone 15 Pro en una escena de contraluz (ventana con exterior brillante):
1. Toma una foto normal (HEIF) y otra en **ProRAW**.
2. Repite de noche en una calle con modo Noche activado y desactivado.
3. Abre las fotos en la PC: la HEIF tal cual, y la ProRAW revelada en darktable o Lightroom móvil. Compara sombras, ruido y halos.
4. Haz la misma escena con la **Sony a6700** en RAW (una sola exposición) y luego con bracketing. Observa cuándo el celular "gana" (comodidad, contraluz rápido) y cuándo la a6700 (rango tonal natural, desenfoque real, movimiento).
5. **Osmo Pocket 4P**: también aplica procesamiento propio; compárala de noche con su sensor de 1 pulgada.

### Cuándo usarlo y cuándo no
Aprovecha el modo automático del celular para escenas rápidas, contraluces y noche sin tripié. Pasa a ProRAW o a la cámara dedicada cuando necesites control fino, un look natural o vas a imprimir grande. Cuidado con depender de la IA para "inventar" detalle: en documental y periodismo importa la fidelidad ([[Ética del retoque]]).

### Errores comunes
- **Creer que ProRAW es "sin procesar"**: ya trae fusión y mapeo de tonos.
- **Juzgar en la pantalla del celular**: el HDR la hace ver más brillante que en otros dispositivos.
- **Exportar sin convertir a sRGB** para web: colores apagados o raros en algunos navegadores.
- **Modo Noche en todo**: la noche deja de parecer noche.

### Para profundizar
- [Apple: usar el modo Noche en el iPhone](https://support.apple.com/es-lamr/102519).
- [[iPhone 15 Pro para foto y video]].
- [[Espacios de color (sRGB, Adobe RGB y Display P3)]].
- Laboratorio: [[Práctica Foto 10 - Panorámica y time-lapse]].

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es la fotografía computacional?
> Usar algoritmos para combinar varias tomas o datos (profundidad, movimiento) y crear una imagen mejor que la que daría una sola exposición del sensor.

> [!question]- Menciona tres funciones computacionales del iPhone 15 Pro.
> Fusión de varias tomas para HDR (Smart HDR), modo Noche, modo Retrato con desenfoque simulado; también Deep Fusion en luz media. Ver [[iPhone 15 Pro para foto y video]].

> [!question]- ¿Qué limita el procesamiento computacional y cómo lo controlas?
> A veces da look "sobreprocesado" (sombras muy abiertas, nitidez excesiva). Usa ProRAW, Estilos fotográficos o una app manual para más control.

> [!question]- ¿Por qué una foto del iPhone puede verse diferente en la pantalla del teléfono que en la PC?
> El iPhone guarda HDR y color Display P3; pantallas o programas sin soporte lo muestran distinto. Ver [[Espacios de color (sRGB, Adobe RGB y Display P3)]].

## Práctica

- [[Práctica Foto 10 - Panorámica y time-lapse]]

