---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# NAT estático

Parte de: [[NAT y PAT]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El **NAT estático** crea un **mapeo fijo uno a uno** entre una dirección interna (inside local) y una dirección pública (inside global). La traducción existe siempre, haya tráfico o no, y funciona **en ambos sentidos**: un equipo de internet puede iniciar una conexión hacia la IP pública y llegará al servidor interno. Es la forma clásica de **publicar un servidor** que vive en una red privada.

### Cómo funciona
1. Se configura la regla `ip nat inside source static <inside local> <inside global>`.
2. Se marcan las interfaces con `ip nat inside` (hacia la LAN o DMZ) y `ip nat outside` (hacia internet).
3. Paquetes que salen del servidor: el router cambia el origen de la IP privada a la pública.
4. Paquetes que llegan desde internet a la IP pública: el router cambia el destino a la IP privada del servidor y los entrega.
5. La IP pública debe enrutarse hacia el router: el proveedor te asigna el bloque y lo envía a tu interfaz outside (si la IP pública es de la misma subred de la interfaz outside, el router responde ARP por ella).

**Variante con puertos (port forwarding)**: en lugar de mapear toda la IP, se mapea un puerto concreto, por ejemplo `ip nat inside source static tcp 192.168.1.5 443 203.0.113.2 443`. Así una sola IP pública sirve para varios servicios internos distintos (443 a un servidor, 22 a otro) y no se exponen todos los puertos del servidor.

**Desventaja principal**: consume **una IP pública por cada servidor** (en la versión de IP completa), y las IPv4 públicas son escasas y caras. Además, expone el servidor completo a internet si no hay un firewall que limite los puertos. Por eso en la práctica se publica con mapeos de puerto, con un proxy inverso o balanceador, y siempre detrás de reglas de firewall.

### Ejemplo
Publicar el proxy inverso que da acceso a la API de Itzamna.Dionisio, `192.168.50.10` en la DMZ, como `203.0.113.10`:

```text
R1(config)# ip nat inside source static 192.168.50.10 203.0.113.10
R1(config)# interface g0/2
R1(config-if)# description DMZ
R1(config-if)# ip nat inside
R1(config-if)# interface g0/1
R1(config-if)# description Proveedor
R1(config-if)# ip nat outside
R1(config-if)# end
R1# show ip nat translations
Pro Inside global     Inside local       Outside local   Outside global
--- 203.0.113.10      192.168.50.10      ---             ---
```

La entrada sin protocolo ni puertos es la regla estática permanente. Cuando haya conexiones aparecerán además entradas con `tcp` y puertos.

Solo HTTPS, con mapeo de puerto:

```text
R1(config)# ip nat inside source static tcp 192.168.50.10 443 203.0.113.10 443
```

Y una ACL extendida en la interfaz outside (o un firewall) que solo deje entrar TCP 443 hacia `203.0.113.10`.

En Azure el concepto equivalente es asociar una IP pública a un balanceador con una regla de entrada, o reglas DNAT en Azure Firewall.

### Cuándo usarlo y cuándo no
Úsalo para servidores que deben ser alcanzables desde internet con una IP fija: VPN, correo, servicios web, cámaras de un NVR (con mucho cuidado). Si solo necesitas que los equipos internos **salgan** a internet, usa PAT. Si tienes muchos servicios web, mejor un proxy inverso con una sola IP pública que enrute por nombre.

### Errores comunes
- **Olvidar marcar `ip nat inside` en la interfaz del servidor** (por ejemplo, la DMZ).
- **Publicar el servidor completo** sin limitar puertos con ACL o firewall.
- **IP pública no enrutada** hacia el router: el proveedor no la entrega.
- **Mapear la misma IP pública** a dos servidores sin distinguir puertos: conflicto.

### Para profundizar
- [RFC 3022: NAT tradicional](https://www.rfc-editor.org/rfc/rfc3022)
- [RFC 2663: terminología de NAT](https://www.rfc-editor.org/rfc/rfc2663)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es NAT estático?
> Una traducción fija uno a uno entre una IP privada y una pública. Se usa para publicar servidores internos.

> [!question]- Publica el servidor 192.168.1.5 como 203.0.113.3.
> `ip nat inside source static 192.168.1.5 203.0.113.3` (más `ip nat inside/outside` en las interfaces).

> [!question]- ¿Cuál es su desventaja?
> Consume una IP pública por cada host traducido.

## Práctica

- [[Lab 1 - DHCP y NAT-PAT en Packet Tracer]]

