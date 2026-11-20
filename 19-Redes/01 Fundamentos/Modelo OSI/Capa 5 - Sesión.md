---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Capa 5 - Sesión

Parte de: [[Modelo OSI]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La capa de sesión se encarga de **abrir, mantener, sincronizar y cerrar diálogos** entre dos aplicaciones. Responde preguntas como: ¿quién habla ahora?, ¿en qué punto vamos?, ¿cómo retomamos si se cortó? En el [[Modelo OSI]] es una capa separada; en la pila TCP/IP real no existe un protocolo "de sesión" puro y sus funciones se reparten entre la aplicación, bibliotecas como TLS y la propia [[Capa 4 - Transporte]].

### Cómo funciona
Las funciones clásicas que OSI asigna a esta capa son:
1. **Establecimiento**: negociar que dos procesos van a conversar y con qué parámetros.
2. **Control del diálogo**: decidir si la conversación es simplex, half dúplex (por turnos) o full dúplex.
3. **Sincronización y puntos de control**: marcar avances para que, si algo falla, no haya que empezar de cero.
4. **Cierre ordenado**: terminar la conversación sin perder datos pendientes.

Diferencia clave con TCP: TCP mantiene una **conexión** de transporte entre dos sockets; una **sesión** puede sobrevivir a varias conexiones. Si tu Wi-Fi se cae y la app abre una conexión TCP nueva, la sesión lógica puede continuar.

### Ejemplo
Ejemplos reales donde ves funciones de sesión:
- **Reanudación de sesión TLS**: cuando el navegador vuelve a conectarse a la API de Itzamna.Dionisio, puede presentar un ticket de sesión para no repetir todo el intercambio de llaves. Eso ahorra tiempo y CPU.
- **Sesiones de aplicación**: el login en Xolotl.Hecate emite un JWT y un refresh token; la "sesión" del usuario dura horas aunque se abran y cierren decenas de conexiones TCP. Es una función de sesión implementada en la capa de aplicación.
- **Descargas reanudables**: HTTP con encabezados `Range` permite continuar una descarga grande desde el byte donde se cortó; es un punto de control.
- **Protocolos con "sesión" en el nombre**: NetBIOS Session Service, RPC o SIP (que establece, modifica y termina sesiones de voz).
- **Una videollamada**: se establece la llamada, se coordina quién transmite qué flujo y, si alguien se desconecta un momento, se reincorpora a la misma reunión.

Para el examen CCNA no se piden comandos de capa 5; lo importante es reconocer la función y saber que en TCP/IP vive dentro de la capa de aplicación.

```text
Navegador ----(conexión TCP 1)----> API   inicia sesión TLS, recibe ticket
Navegador ----(conexión TCP 2)----> API   presenta ticket, reanuda sin handshake completo
```

### Cuándo usarlo y cuándo no
Piensa en "capa 5" cuando el problema es de **estado de la conversación**: sesiones que expiran antes de tiempo, usuarios que pierden su carrito al cambiar de red, balanceadores que mandan cada petición a un servidor distinto sin afinidad, o tokens que no se renuevan. No busques un comando de router para esto: casi siempre se resuelve en la aplicación, en el balanceador (afinidad de sesión) o en la configuración de TLS.

### Errores comunes
- **Confundir sesión con conexión TCP**: una sesión puede usar muchas conexiones; cerrar una conexión no siempre cierra la sesión.
- **Buscar un protocolo de capa 5 en cada captura**: en Wireshark verás TCP y luego TLS o HTTP; no aparecerá una capa "Sesión" separada.
- **Ignorar los tiempos de espera**: firewalls y NAT cierran entradas inactivas; si la app no envía keepalives, la sesión "muere" sin aviso.
- **Guardar estado de sesión en un solo servidor** detrás de un balanceador sin afinidad: cada petición puede caer en otro nodo.

### Para profundizar
- [ISO/IEC 7498-1 (modelo de referencia OSI)](https://www.iso.org/standard/20269.html)
- [RFC 8446: TLS 1.3 (reanudación con PSK)](https://www.rfc-editor.org/rfc/rfc8446)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace la capa de sesión?
> Establece, mantiene y termina el diálogo entre aplicaciones; puede manejar puntos de control para reanudar transferencias.

> [!question]- Da un ejemplo de función de sesión en el mundo real.
> Reanudar una descarga grande desde el punto donde se cortó, o mantener una sesión RPC/NetBIOS abierta entre cliente y servidor.

