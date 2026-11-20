---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# IPsec

Parte de: [[VPN (sitio a sitio y acceso remoto, IPsec y SSL)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**IPsec** es un conjunto de protocolos estándar del IETF que protege el tráfico **en la capa de red (capa 3)**: cifra y autentica paquetes IP. Es la tecnología más usada para VPN sitio a sitio y también para algunas VPN de acceso remoto. Como opera en capa 3, protege cualquier protocolo que viaje sobre IP sin que las aplicaciones tengan que hacer nada.

### Cómo funciona
**Componentes**:
- **IKE** (Internet Key Exchange, hoy **IKEv2**): negocia los parámetros de seguridad, autentica a los extremos (con **llave precompartida** o **certificados**) y genera las llaves de sesión con **Diffie-Hellman**. Usa **UDP 500**, y **UDP 4500** cuando hay NAT de por medio (**NAT-T**).
- **ESP** (Encapsulating Security Payload, protocolo IP 50): **cifra** el contenido y además lo **autentica**. Es el que se usa en la práctica.
- **AH** (Authentication Header, protocolo IP 51): autentica e integra el paquete completo pero **no cifra**; no funciona a través de NAT. Casi no se usa.
- **SA** (Security Association): el acuerdo unidireccional con algoritmos, llaves y tiempos de vida. Hay una SA de IKE y pares de SA de IPsec (una por sentido).

**Fases** (en terminología clásica):
1. **IKE SA**: los extremos se autentican y crean un canal seguro de control.
2. **IPsec SA (child SA)**: sobre ese canal acuerdan cómo proteger el tráfico de datos (qué redes, qué algoritmos) y crean las SA de ESP.

**Modos**:
- **Modo túnel**: se cifra **el paquete IP completo** (encabezado original incluido) y se agrega un **encabezado IP nuevo** con las IP de los gateways VPN. Es el modo de las VPN sitio a sitio: oculta las IP internas.
- **Modo transporte**: se protege solo la **carga útil**; el encabezado IP original se conserva. Se usa de host a host o combinado con GRE (el túnel GRE ya aporta el encabezado externo).

**Servicios de seguridad**:
- **Confidencialidad**: cifrado (AES-GCM o AES-CBC).
- **Integridad**: HMAC con SHA-2 (o integrado en GCM).
- **Autenticación de origen**: PSK o certificados.
- **Anti-replay**: números de secuencia para rechazar paquetes repetidos.
- **Perfect Forward Secrecy** (PFS) si se negocia un nuevo Diffie-Hellman para cada SA.

```text
Modo túnel:      [IP nueva][ESP][ IP original | TCP | Datos ](cifrado)[ESP trailer][ICV]
Modo transporte: [IP original][ESP][ TCP | Datos ](cifrado)[ESP trailer][ICV]
```

### Ejemplo
Para un túnel con Azure o con otra oficina, ambos lados deben coincidir en: versión de IKE, algoritmos de cifrado e integridad, grupo Diffie-Hellman, tiempo de vida, PSK o certificados y redes protegidas (o usar un túnel basado en rutas). Verificación en Cisco:

```text
R1# show crypto ikev2 sa detailed
R1# show crypto ipsec sa peer 198.51.100.20
    #pkts encaps: 1520, #pkts encrypt: 1520
    #pkts decaps: 1498, #pkts decrypt: 1498
    local crypto endpt.: 203.0.113.2, remote crypto endpt.: 198.51.100.20
     inbound esp sas: spi: 0x9C1D2A11 ... transform: esp-aes 256 esp-sha256-hmac
```

En el firewall de borde deben permitirse UDP 500, UDP 4500 y el protocolo ESP (50) hacia el gateway VPN. Si hay NAT en algún extremo, ESP viaja encapsulado en UDP 4500.

Para identificar el tráfico IPsec en [[Wireshark (captura y análisis de tráfico)]] usa los filtros `isakmp` (negociación IKE) y `esp`.

### Cuándo usarlo y cuándo no
IPsec es la opción por defecto para sitio a sitio entre equipos de distintos fabricantes y con la nube. Para acceso remoto es buena opción con IKEv2 (clientes nativos en Windows, macOS, iOS), aunque redes restrictivas (hoteles) que bloquean UDP 500/4500 favorecen las VPN SSL sobre TCP 443. Para proteger una aplicación concreta, TLS suele ser más simple.

### Errores comunes
- **Parámetros distintos en cada extremo** (la causa más común de que no suba la fase 1 o 2).
- **Bloquear ESP o UDP 4500** en el firewall.
- **Algoritmos obsoletos** (DES, 3DES, MD5, grupo DH 1 o 2).
- **Redes protegidas que no coinciden** exactamente en ambos lados (en VPN basadas en políticas).

### Para profundizar
- [RFC 4301: arquitectura de seguridad para IP](https://www.rfc-editor.org/rfc/rfc4301)
- [RFC 4303: ESP](https://www.rfc-editor.org/rfc/rfc4303)
- [RFC 7296: IKEv2](https://www.rfc-editor.org/rfc/rfc7296)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué componentes tiene IPsec?
> **IKE** (negociación y llaves; UDP 500, o 4500 con NAT-T), **ESP** (protocolo IP 50: cifrado e integridad) y **AH** (protocolo IP 51: solo integridad, no funciona con NAT).

> [!question]- ¿Qué diferencia hay entre modo túnel y modo transporte?
> **Túnel**: cifra todo el paquete original y agrega un encabezado IP nuevo (típico entre gateways). **Transporte**: protege solo la carga útil y mantiene el encabezado original (de host a host).

> [!question]- ¿Qué servicios de seguridad aporta?
> Confidencialidad (cifrado, p. ej. AES), integridad (hash, p. ej. SHA-2), autenticación (PSK o certificados) y protección contra repetición. El intercambio de llaves usa Diffie-Hellman.

