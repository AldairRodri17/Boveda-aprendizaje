---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
contenido: completo
---
# Modelo TCP-IP

Parte de: [[19-Redes/01 Fundamentos/_Índice Fundamentos|Fundamentos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El modelo TCP/IP es la arquitectura que **realmente usa internet**. Nació de ARPANET y está definido por los RFC del IETF. A diferencia del [[Modelo OSI]], que se diseñó como referencia teórica, TCP/IP se describe a partir de protocolos que ya funcionaban. Su nombre viene de sus dos protocolos centrales: TCP (transporte) e IP (red).

### Cómo funciona
La versión más citada (RFC 1122) tiene **cuatro capas**; muchos libros, incluido el material de Cisco, usan una versión "actualizada" de cinco que separa la capa física:

| TCP/IP (4 capas) | TCP/IP (5 capas) | Equivalente OSI | Protocolos |
|---|---|---|---|
| Aplicación | Aplicación | 7, 6 y 5 | HTTP, DNS, DHCP, SSH, TLS |
| Transporte | Transporte | 4 | TCP, UDP |
| Internet | Red | 3 | IPv4, IPv6, ICMP |
| Acceso a la red (enlace) | Enlace de datos | 2 | Ethernet, 802.11, PPP, ARP* |
| | Física | 1 | Cobre, fibra, radio |

*ARP se suele ubicar entre enlace e internet, porque usa tramas Ethernet para resolver direcciones IP.

Diferencias principales con OSI:
- **Menos capas**: TCP/IP agrupa sesión, presentación y aplicación en una sola capa de aplicación, porque en la práctica esas funciones las implementa cada aplicación o biblioteca.
- **Orientado a protocolos reales**: OSI define el modelo y luego protocolos (casi en desuso); TCP/IP es al revés.
- **Capa de acceso a la red genérica**: TCP/IP no especifica cómo funciona la red local; se apoya en estándares IEEE como Ethernet y Wi-Fi.

El flujo de encapsulamiento es igual que en OSI: cada capa agrega su encabezado al bajar y lo quita al subir (ver [[Encapsulamiento y PDUs]]).

### Ejemplo
Cuando Ocelotl.Hermes manda un correo de notificación a través de un servidor SMTP:

```text
Aplicación : SMTP  "MAIL FROM:<no-reply@...>"           (más TLS con STARTTLS)
Transporte : TCP   puerto origen 51544 -> destino 587
Internet   : IP    10.0.2.15 -> 203.0.113.25
Acceso red : Ethernet  MAC del servidor -> MAC del gateway, luego bits por el cable
```

Si lo miras en [[Wireshark (captura y análisis de tráfico)]], verás estas mismas capas en el panel de detalle: "Ethernet II", "Internet Protocol Version 4", "Transmission Control Protocol" y "Simple Mail Transfer Protocol".

### Cuándo usarlo y cuándo no
Usa TCP/IP para hablar de **implementaciones**: cómo se configura una pila, qué protocolo se usa, cómo viaja un paquete en internet. Usa OSI cuando necesitas más granularidad para **explicar o diagnosticar** (distinguir capa 1 de capa 2, por ejemplo). En el examen CCNA te pueden preguntar por cualquiera de los dos y por la correspondencia entre ambos. Un buen hábito: al leer sobre un protocolo nuevo, ubícalo en ambos modelos.

### Errores comunes
- **Decir que TCP/IP "reemplazó" a OSI**: conviven; uno es implementación y el otro referencia.
- **Ubicar Ethernet en la capa Internet**: Ethernet es acceso a la red (capas 1 y 2); IP es la capa Internet.
- **Contar capas distinto sin aclarar**: si hablas de "la capa 4 de TCP/IP" puede haber confusión entre el modelo de 4 y el de 5; di el nombre (transporte).
- **Olvidar que la capa de aplicación de TCP/IP incluye cifrado y formato**: TLS y JSON viven ahí.

### Para profundizar
- [RFC 1122: requisitos para hosts de internet (capas de comunicación)](https://www.rfc-editor.org/rfc/rfc1122)
- [RFC 1123: requisitos para hosts (aplicación y soporte)](https://www.rfc-editor.org/rfc/rfc1123)
- [RFC 791: Internet Protocol](https://www.rfc-editor.org/rfc/rfc791)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cuáles son las capas del modelo TCP/IP?
> 4 capas: **Acceso a la red** (OSI 1-2), **Internet** (OSI 3), **Transporte** (OSI 4) y **Aplicación** (OSI 5-7). Algunos textos lo presentan con 5 capas separando física y enlace.

> [!question]- ¿Qué diferencia principal tiene con OSI?
> TCP/IP nació de protocolos reales y agrupa sesión, presentación y aplicación en una sola capa; OSI es un modelo teórico más detallado.

> [!question]- ¿En qué capa TCP/IP están IP, TCP y Ethernet?
> IP → Internet · TCP → Transporte · Ethernet → Acceso a la red.

