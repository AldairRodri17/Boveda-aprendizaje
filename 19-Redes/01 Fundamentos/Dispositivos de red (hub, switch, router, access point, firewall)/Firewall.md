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
# Firewall

Parte de: [[Dispositivos de red (hub, switch, router, access point, firewall)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **firewall** es un dispositivo (físico, virtual o software) que **controla el tráfico entre zonas de red** según una política de seguridad: decide qué se permite y qué se bloquea. Se ubica en las fronteras, por ejemplo entre la LAN e internet, entre la LAN y una DMZ, o entre segmentos internos. Su principio base es "negar todo lo que no se permita explícitamente".

### Cómo funciona
Un firewall evalúa cada flujo con reglas que combinan zona de origen y destino, IP, puertos, protocolo y, en equipos modernos, usuario y aplicación. Hay tres generaciones (detalladas en [[Firewalls (stateless, stateful y de nueva generación)]]):
- **Filtrado de paquetes sin estado**: revisa cada paquete aislado. Una ACL de router es un ejemplo.
- **Stateful (con estado)**: lleva una **tabla de conexiones**. Si un equipo interno inicia una conexión a internet, el firewall recuerda el flujo y permite automáticamente las respuestas, pero bloquea conexiones nuevas iniciadas desde afuera.
- **NGFW (nueva generación)**: además identifica aplicaciones (no solo puertos), usuarios, inspecciona TLS si se configura, incluye IPS y filtrado de URL.

**Firewall frente a ACL en un router**: una ACL estándar o extendida en Cisco IOS es un filtro sin estado; para permitir respuestas tienes que escribir reglas de regreso o usar trucos como `established`. Un firewall stateful hace eso de forma natural, maneja zonas, registra eventos y suele ofrecer VPN e inspección profunda. Las ACL son útiles y rápidas, pero no sustituyen a un firewall en el perímetro.

### Ejemplo
Política típica para la red donde corre Itzamna.Dionisio:

| Origen | Destino | Servicio | Acción |
|---|---|---|---|
| Internet | DMZ: proxy inverso 203.0.113.10 | TCP 443 | Permitir |
| DMZ: proxy | LAN: API 10.0.20.15 | TCP 8443 | Permitir |
| LAN: API | LAN: SQL Server 10.0.30.5 | TCP 1433 | Permitir |
| LAN usuarios | Internet | TCP 80/443, UDP 53 a DNS internos | Permitir |
| Cualquiera | Cualquiera | Cualquiera | **Denegar y registrar** |

En el propio servidor también hay firewall de host. Ejemplo en Windows con PowerShell:

```powershell
# Permitir HTTPS entrante solo desde la subred del proxy
New-NetFirewallRule -DisplayName "API HTTPS desde DMZ" -Direction Inbound `
  -Protocol TCP -LocalPort 8443 -RemoteAddress 10.0.10.0/24 -Action Allow
```

En Azure la idea equivalente son los NSG y Azure Firewall (ver [[Redes en la nube (VNet, subnets, NSG y peering)]]).

### Cuándo usarlo y cuándo no
Siempre en el perímetro hacia internet y entre zonas de distinta confianza (usuarios, servidores, invitados, IoT). Las ACL de router o los NSG sirven para filtrado rápido interno. Un firewall no sustituye a parchar servidores, usar MFA ni a cifrar: es una capa más en la defensa en profundidad (ver [[Segmentación y DMZ]] y [[Zero Trust en redes]]).

### Errores comunes
- **Reglas "any any" temporales** que se quedan para siempre.
- **Orden de reglas incorrecto**: una regla amplia arriba anula a las específicas de abajo.
- **No registrar los bloqueos**: sin logs no puedes diagnosticar ni detectar ataques.
- **Confiar solo en el perímetro**: un equipo comprometido dentro de la LAN pasa por encima.

### Para profundizar
- [NIST SP 800-41 Rev. 1: guía de firewalls y políticas](https://csrc.nist.gov/pubs/sp/800/41/r1/final)
- [Microsoft Learn: Firewall de Windows](https://learn.microsoft.com/windows/security/operating-system-security/network-security/windows-firewall/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace un firewall?
> Filtra el tráfico entre zonas de confianza según reglas: permite lo autorizado y bloquea lo demás.

> [!question]- ¿Qué diferencia hay entre un firewall y una ACL en un router?
> La ACL básica es *stateless* (revisa cada paquete sin contexto). Un firewall normalmente es *stateful*: recuerda las conexiones y deja pasar automáticamente las respuestas.

