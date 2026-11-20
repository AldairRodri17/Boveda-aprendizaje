---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Seguridad Wi-Fi (WPA2, WPA3 y 802.1X)

Parte de: [[19-Redes/06 Inalámbrico/_Índice Inalámbrico|Inalámbrico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Como la señal de radio llega a cualquiera dentro del alcance, la seguridad Wi-Fi debe resolver dos cosas: **autenticar** quién se conecta y **cifrar** el tráfico en el aire. Los mecanismos actuales son **WPA2** y **WPA3** (certificaciones de la Wi-Fi Alliance basadas en IEEE 802.11i y posteriores), en modo **Personal** (una contraseña compartida) o **Enterprise** (credenciales individuales con **802.1X** y un servidor RADIUS).

### Cómo funciona
**Personal frente a Enterprise**:
- **Personal (PSK / SAE)**: todos usan la misma frase de paso. Fácil, ideal para casa o invitados. Si alguien se va de la empresa, hay que cambiar la clave a todos; y quien conoce la clave puede, con WPA2, intentar descifrar lo capturado.
- **Enterprise (802.1X)**: cada usuario o dispositivo se autentica con sus propias credenciales (usuario y contraseña, certificado). Se puede revocar a una persona sin afectar al resto, asignar VLAN por usuario y auditar quién se conectó.

**WPA2**: cifrado **AES-CCMP**. En Personal usa una PSK y el *handshake* de cuatro vías, que es vulnerable a ataques de diccionario fuera de línea si la frase es débil.

**WPA3** mejora:
- **SAE** (Simultaneous Authentication of Equals) en lugar de PSK en modo Personal: resiste ataques de diccionario fuera de línea y da *forward secrecy* (capturar hoy no sirve para descifrar mañana aunque se filtre la clave).
- **PMF** (Protected Management Frames) obligatorio: dificulta ataques de desautenticación.
- Modo **Enterprise de 192 bits** para entornos de alta seguridad.
- **Enhanced Open (OWE)**: cifra redes abiertas sin contraseña (cafeterías), aunque no autentica.
- Existe un modo de transición WPA2/WPA3 para convivir con clientes antiguos.

**Roles de 802.1X**:
1. **Suplicante**: el cliente (laptop, iPhone) que quiere conectarse.
2. **Autenticador**: el AP o el WLC (o el switch, en redes cableadas), que bloquea el acceso hasta que la autenticación termina y pasa los mensajes EAP.
3. **Servidor de autenticación**: un **RADIUS** (Cisco ISE, Microsoft NPS, FreeRADIUS) que valida credenciales contra el directorio y responde aceptar o rechazar, a veces con una VLAN o una política.
Los métodos EAP más comunes son **PEAP-MSCHAPv2** (usuario y contraseña dentro de un túnel TLS) y **EAP-TLS** (certificados en ambos lados, el más fuerte).

**Nunca uses**: **WEP** (roto desde hace años), **WPA original con TKIP**, redes abiertas sin OWE para datos corporativos, y **WPS** (el PIN de 8 dígitos se puede forzar). Tampoco confíes en ocultar el SSID ni en filtrar por MAC como seguridad: se evaden fácilmente.

### Ejemplo
Diseño para la oficina:

| SSID | Seguridad | VLAN | Quién |
|---|---|---|---|
| Itzamna-Corp | WPA3-Enterprise (EAP-TLS o PEAP) con RADIUS | 10 | Empleados |
| Itzamna-IoT | WPA2/WPA3-Personal con clave larga | 40 | Impresoras, pantallas |
| Itzamna-Invitados | WPA3-Personal (o OWE) + aislamiento de clientes | 30 | Visitas, solo internet |

Configuración de un servidor RADIUS en un WLC o router (estilo IOS-XE):

```text
WLC(config)# aaa new-model
WLC(config)# radius server NPS1
WLC(config-radius-server)# address ipv4 10.20.0.15 auth-port 1812 acct-port 1813
WLC(config-radius-server)# key <secreto-compartido>
WLC(config)# aaa group server radius GRUPO-RADIUS
WLC(config-sg-radius)# server name NPS1
WLC(config)# aaa authentication dot1x AUTH-DOT1X group GRUPO-RADIUS
```

En casa: WPA3-Personal (o WPA2/WPA3 de transición si tienes equipos viejos), frase de paso larga, WPS desactivado y firmware actualizado del router. Tu DJI Mini 4K y la Osmo Pocket usan sus propios enlaces cifrados con el control o la app; no dependen de la seguridad de tu Wi-Fi, pero el iPhone que los controla sí.

En [[Cisco Packet Tracer]] puedes configurar WPA2-PSK en un AP y, con un servidor AAA, WPA2-Enterprise.

### Cuándo usarlo y cuándo no
Enterprise para cualquier red corporativa con usuarios; Personal para casa, invitados con clave rotada o dispositivos que no soportan 802.1X (aisla esos dispositivos en su VLAN). WPA3 siempre que todos los clientes lo soporten.

### Errores comunes
- **PSK corta o compartida en un cartel**.
- **Dejar WPS activado**.
- **No validar el certificado del servidor** en PEAP: permite ataques de AP falso que roban credenciales.
- **Invitados en la misma VLAN que los empleados**.

### Para profundizar
- [Wi-Fi Alliance: WPA3 and Wi-Fi Enhanced Open Deployment Guide](https://wi-fi.org/system/files/WPA3%20and%20Wi-Fi%20Enhanced%20Open%20Deployment%20Guide_v1.0.pdf)
- [IEEE 802.1X, IEEE 802.1 Working Group](https://www.ieee802.org/1/)
- [RFC 5216: EAP-TLS](https://www.rfc-editor.org/rfc/rfc5216)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre modo Personal y Enterprise?
> **Personal (PSK)**: una contraseña compartida para todos. **Enterprise**: cada usuario se autentica con 802.1X contra un servidor RADIUS.

> [!question]- ¿Qué mejora WPA3 frente a WPA2?
> En Personal cambia PSK por **SAE**, resistente a ataques de diccionario fuera de línea; hace obligatorio PMF (protección de tramas de gestión) y ofrece un modo Enterprise de 192 bits.

> [!question]- ¿Cuáles son los tres roles de 802.1X?
> **Suplicante** (cliente), **autenticador** (AP o WLC) y **servidor de autenticación** (RADIUS). Los mensajes de autenticación son EAP.

> [!question]- ¿Qué opciones nunca deberías usar?
> **WEP**, **WPA con TKIP** y **WPS** (el PIN se puede adivinar por fuerza bruta).

## Práctica

- [[Lab 1 - Wi-Fi básico en Packet Tracer]]

