---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Capa 7 - Aplicación

Parte de: [[Modelo OSI]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La capa de aplicación es la más cercana al usuario: define los **protocolos que usan las aplicaciones** para intercambiar información a través de la red. Ojo: la capa 7 no es "el navegador" ni "Outlook", sino los protocolos que esos programas hablan (HTTP, SMTP, DNS). Es donde se describen las reglas de mensajes, comandos y respuestas de cada servicio.

### Cómo funciona
Una aplicación cliente crea un mensaje según el protocolo (por ejemplo, una petición HTTP con método, ruta y encabezados), lo entrega al sistema operativo indicando el puerto destino y el protocolo de transporte, y el resto de la pila se encarga de encapsularlo. El servidor escucha en un puerto conocido, recibe el mensaje y responde con el mismo protocolo.

Protocolos de capa 7 que debes conocer para CCNA con su transporte y puerto:

| Protocolo | Uso | Transporte y puerto |
|---|---|---|
| HTTP / HTTPS | Web y APIs | TCP 80 / TCP 443 |
| DNS | Resolución de nombres | UDP 53 (y TCP 53) |
| DHCP | Asignación de IP | UDP 67 servidor, 68 cliente |
| SSH | Administración remota segura | TCP 22 |
| Telnet | Administración remota sin cifrado | TCP 23 |
| FTP | Transferencia de archivos | TCP 20 y 21 |
| TFTP | Transferencia simple (imágenes IOS) | UDP 69 |
| SMTP | Envío de correo | TCP 25 (587 para clientes) |
| POP3 / IMAP | Lectura de correo | TCP 110 / TCP 143 |
| SNMP | Monitoreo | UDP 161 (traps 162) |
| Syslog | Registros | UDP 514 |
| NTP | Hora | UDP 123 |

Los servicios de red se ven a detalle en [[DNS en la red (resolución, registros y servidores)]], [[DHCP]], [[NTP]], [[SNMP y Syslog]] y [[FTP, SFTP y TFTP]].

### Ejemplo
Un usuario dice "no abre Google". Pruebas desde su PC:

```text
C:\> ping 8.8.8.8
Respuesta desde 8.8.8.8: bytes=32 tiempo=14ms TTL=117
C:\> ping google.com
La solicitud de ping no pudo encontrar el host google.com.
C:\> nslookup google.com
*** No se puede encontrar el servidor para la dirección 192.168.1.53: Timed out
```

La conectividad IP funciona (capas 1 a 3 sanas), pero la resolución de nombres falla: el problema está en la capa 7, en el servicio DNS (servidor caído, IP de DNS mal configurada o bloqueada). La solución es corregir el DNS del cliente (`ipconfig /all` para verlo) o del servidor DHCP que lo entrega.

Como desarrollador lo vives a diario: un `500` o un `401` de tu API es un error de capa 7; la red hizo su trabajo y el mensaje llegó.

### Cuándo usarlo y cuándo no
Diagnostica en capa 7 cuando la conectividad hasta la IP y el puerto está comprobada (`ping`, `Test-NetConnection host -Port 443`, `curl -v`) y aun así el servicio falla: errores HTTP, certificados, autenticación, DNS que responde mal, o un servicio que no escucha. Si ni siquiera llega el ping, baja de capa.

### Errores comunes
- **Confundir la aplicación con el protocolo**: Chrome no es capa 7; HTTP sí.
- **Olvidar que DNS también usa TCP**: transferencias de zona y respuestas grandes van por TCP 53.
- **Asumir que "si hay ping hay servicio"**: el servidor puede responder ICMP y tener el puerto 443 cerrado o el servicio detenido.
- **Usar Telnet o FTP sin cifrado en producción**: viajan credenciales en texto claro; usa SSH y SFTP.

### Para profundizar
- [IANA: registro de puertos y servicios](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml)
- [RFC 9110: semántica de HTTP](https://www.rfc-editor.org/rfc/rfc9110)
- [RFC 1035: DNS](https://www.rfc-editor.org/rfc/rfc1035)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace la capa de aplicación?
> Ofrece servicios de red a las aplicaciones del usuario: es la interfaz entre el software y la red. No es la aplicación en sí (el navegador), sino el protocolo (HTTP).

> [!question]- Nombra cinco protocolos de capa 7 con su puerto.
> HTTP 80/TCP · HTTPS 443/TCP · DNS 53/UDP (y TCP) · SSH 22/TCP · DHCP 67-68/UDP · SMTP 25/TCP.

> [!question]- Si `ping 8.8.8.8` funciona pero `ping google.com` no, ¿en qué capa está el problema?
> La conectividad de capa 3 está bien; falla la **resolución DNS** (capa 7). Revisa el servidor DNS configurado en el equipo.

