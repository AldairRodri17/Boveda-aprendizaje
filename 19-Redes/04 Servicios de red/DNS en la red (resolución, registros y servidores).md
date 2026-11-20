---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# DNS en la red (resolución, registros y servidores)

Parte de: [[19-Redes/04 Servicios de red/_Índice Servicios de red|Servicios de red]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**DNS** (Domain Name System) traduce **nombres** que entendemos (`dev.azure.com`, `api.itzamna.local`) a **direcciones IP** que la red usa, y también otros datos (servidores de correo, alias, texto de verificación). Es una base de datos distribuida y jerárquica: nadie la tiene completa, cada zona la administra su dueño. Si DNS falla, para el usuario "no hay internet" aunque la red IP esté perfecta.

### Cómo funciona
**Roles**:
- **Cliente (stub resolver)**: el sistema operativo de tu PC; pregunta y espera una respuesta completa.
- **Resolver recursivo**: el servidor que tu PC usa (el del router, del proveedor, el de la empresa o uno público). Hace todo el trabajo de preguntar por ti y guarda en **caché** las respuestas durante su TTL.
- **Servidores autoritativos**: los que tienen la información oficial de una zona. Los **raíz** saben quién maneja cada TLD (`.com`, `.mx`), los del TLD saben quién maneja cada dominio, y los autoritativos del dominio tienen los registros.

**Resolución recursiva** de `api.ejemplo.com`:
1. El PC pregunta a su resolver.
2. Si no está en caché, el resolver pregunta a un servidor raíz → "pregunta a los de `.com`".
3. Pregunta a `.com` → "pregunta a los NS de `ejemplo.com`".
4. Pregunta al autoritativo de `ejemplo.com` → "`api.ejemplo.com` es `203.0.113.40`".
5. El resolver responde al PC y guarda en caché.

Transporte: **UDP 53** para consultas normales; **TCP 53** para respuestas grandes y transferencias de zona entre servidores. También existen DNS sobre TLS (TCP 853) y sobre HTTPS (443).

**Tipos de registro** principales:

| Registro | Uso |
|---|---|
| A | Nombre → IPv4 |
| AAAA | Nombre → IPv6 |
| CNAME | Alias de otro nombre |
| MX | Servidores de correo del dominio (con prioridad) |
| NS | Servidores autoritativos de la zona |
| PTR | IP → nombre (resolución inversa) |
| TXT | Texto libre: SPF, verificación de dominio |
| SOA | Datos de la zona (servidor primario, serial, tiempos) |
| SRV | Ubicación de servicios (Active Directory, SIP) |

### Ejemplo
Diagnóstico desde Windows:

```powershell
nslookup api.itzamna.local
Resolve-DnsName dev.azure.com -Type A
Resolve-DnsName itzamna.mx -Type MX
ipconfig /displaydns      # caché local
ipconfig /flushdns        # vaciarla tras un cambio
```

En un router Cisco:

```text
R1(config)# ip domain-name itzamna.local
R1(config)# ip name-server 192.168.20.53 1.1.1.1
R1(config)# ip domain-lookup
R1# ping api.itzamna.local
```

**`no ip domain-lookup`**: por defecto IOS intenta resolver por DNS cualquier palabra que no reconozca como comando. Si te equivocas al escribir (`shwo run`), el router intenta "conectarse" a un host llamado `shwo` y te deja esperando varios segundos con `Translating "shwo"...`. En laboratorios se usa `no ip domain-lookup` para evitarlo. En producción, si el router necesita resolver nombres (para NTP o syslog por nombre), es mejor dejarlo activo y usar `transport preferred none` en las líneas de consola y VTY.

Para el desarrollo con la API de Itzamna.Dionisio en Azure, un registro CNAME `api.itzamna.mx` apunta al nombre del App Service o del Front Door, y un TXT valida la propiedad del dominio para el certificado.

### Cuándo usarlo y cuándo no
Usa DNS para todo lo que tenga que encontrarse por nombre; nunca codifiques IPs en las aplicaciones. Internamente, usa una zona privada (Active Directory, Azure Private DNS) y reenvía lo externo a resolvers confiables. Usa TTL cortos antes de una migración y largos en registros estables.

### Errores comunes
- **Diagnosticar "no hay internet" sin probar `ping 8.8.8.8`** frente a `ping google.com`.
- **Olvidar la caché**: un cambio de registro tarda en propagarse según el TTL.
- **Abrir solo UDP 53** en el firewall.
- **CNAME en el apex** del dominio (`itzamna.mx` sin subdominio): no está permitido por el estándar junto con otros registros; usa A/AAAA o registros alias del proveedor.

### Para profundizar
- [RFC 1034: conceptos de DNS](https://www.rfc-editor.org/rfc/rfc1034)
- [RFC 1035: implementación y especificación de DNS](https://www.rfc-editor.org/rfc/rfc1035)
- [Microsoft Learn: Azure DNS](https://learn.microsoft.com/azure/dns/dns-overview)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace DNS?
> Traduce nombres (www.ejemplo.com) a direcciones IP y viceversa. Usa UDP 53 para consultas normales y TCP 53 para respuestas grandes y transferencias de zona.

> [!question]- ¿Qué diferencia hay entre un resolver recursivo y un servidor autoritativo?
> El **recursivo** hace todo el trabajo por el cliente (consulta raíz, TLD y autoritativo, y guarda caché). El **autoritativo** tiene los registros oficiales de una zona.

> [!question]- Nombra cinco tipos de registro y su uso.
> **A** (IPv4) · **AAAA** (IPv6) · **CNAME** (alias) · **MX** (servidor de correo) · **PTR** (inverso) · NS (servidores de la zona) · TXT (texto, SPF/verificación).

> [!question]- En un router, ¿para qué sirve `no ip domain-lookup`?
> Evita que IOS intente resolver por DNS cada comando mal escrito (lo que congela la consola unos segundos). Para usar DNS se configura `ip name-server`.

## Práctica

- [[Lab 1 - DHCP y NAT-PAT en Packet Tracer]]
- [[Lab 2 - Wireshark capturando ARP, ICMP y DNS]]

