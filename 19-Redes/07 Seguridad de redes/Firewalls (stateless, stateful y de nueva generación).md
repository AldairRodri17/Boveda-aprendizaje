---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
contenido: completo
---
# Firewalls (stateless, stateful y de nueva generación)

Parte de: [[19-Redes/07 Seguridad de redes/_Índice Seguridad de redes|Seguridad de redes]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **firewall** aplica una política de seguridad decidiendo qué tráfico pasa entre zonas. Según cómo toma esa decisión hay tres generaciones: **sin estado** (stateless), **con estado** (stateful) y **de nueva generación** (NGFW). Esta nota compara cómo funcionan; el papel del firewall como dispositivo está en [[Firewall]].

### Cómo funciona
**Firewall sin estado (filtrado de paquetes)**:
- Evalúa **cada paquete por separado** contra reglas de IP origen y destino, protocolo y puertos.
- No recuerda conexiones. Para permitir respuestas hay que escribir reglas en ambos sentidos (o usar trucos como `established` en TCP), lo que abre más de lo necesario.
- Ejemplos: ACL de routers, NSG básicos de la nube (aunque los NSG de Azure son con estado), filtros en switches.
- Ventaja: muy rápido y simple. Desventaja: fácil de evadir y difícil de mantener con protocolos que abren puertos dinámicos.

**Firewall con estado (stateful)**:
- Mantiene una **tabla de conexiones** (IP y puerto de origen y destino, protocolo, estado TCP, tiempos).
- Cuando un equipo interno inicia una conexión permitida, el firewall registra el flujo y **permite automáticamente el tráfico de regreso** de esa conexión, sin reglas extra. Bloquea paquetes que no pertenecen a ninguna conexión conocida (por ejemplo, un ACK suelto desde internet).
- Entiende protocolos con canales secundarios (FTP, SIP) mediante inspección de aplicación.
- Ejemplos: Cisco ASA, firewall basado en zonas de IOS (ZBFW), el firewall de Windows, iptables/nftables con `conntrack`.

**NGFW (Next-Generation Firewall)**: todo lo anterior más:
- **Identificación de aplicaciones** independientemente del puerto (sabe que es Teams, BitTorrent o una API, aunque todo vaya por 443).
- **Identidad de usuario** (integración con Active Directory o Entra ID): reglas por grupo, no solo por IP.
- **IPS** integrado (prevención de intrusiones con firmas y comportamiento).
- **Inspección TLS** (descifrar y revisar, si se despliega con certificados propios y respetando privacidad).
- **Filtrado de URL**, antimalware, sandboxing y reputación de IP y dominios.
- Ejemplos: Cisco Secure Firewall (Firepower), Palo Alto, Fortinet, Azure Firewall Premium.

**Política base recomendada**: **denegar todo por defecto** y permitir explícitamente solo lo necesario (principio de mínimo privilegio), tanto de entrada como de salida. Registrar lo denegado, revisar las reglas periódicamente y documentar el motivo y el responsable de cada una.

| Característica | Stateless | Stateful | NGFW |
|---|---|---|---|
| Unidad de decisión | Paquete | Conexión | Conexión + aplicación + usuario |
| Tráfico de regreso | Regla explícita | Automático | Automático |
| Capa | 3 y 4 | 3 y 4 (+ algo de 7) | 3 a 7 |
| Rendimiento | Máximo | Alto | Depende de las funciones activas |

### Ejemplo
En una ACL sin estado de un router, para que las PCs naveguen tendrías que permitir de regreso algo como:

```text
R1(config)# ip access-list extended INTERNET-IN
R1(config-ext-nacl)# permit tcp any 192.168.10.0 0.0.0.255 established
R1(config-ext-nacl)# permit udp any eq domain 192.168.10.0 0.0.0.255
R1(config-ext-nacl)# deny ip any any log
```

La segunda línea deja entrar cualquier paquete UDP con origen 53, incluso no solicitado. Un firewall con estado solo dejaría entrar la respuesta a una consulta que realmente salió.

En Linux, el mismo concepto con estado (`nftables`):

```bash
nft add rule inet filtro entrada ct state established,related accept
nft add rule inet filtro entrada tcp dport 443 accept
nft add rule inet filtro entrada drop
```

### Cuándo usarlo y cuándo no
- Stateless: filtrado rápido en el núcleo, protección del plano de control, listas de bloqueo simples.
- Stateful: perímetro de cualquier red, entre zonas internas y en hosts.
- NGFW: perímetro corporativo, salida a internet de usuarios, segmentos con datos sensibles.
Un NGFW no reemplaza parches, MFA ni el endurecimiento de servidores.

### Errores comunes
- **Permitir "any any" de salida**: facilita la exfiltración y el control de malware.
- **Reglas sin dueño ni fecha** que nadie se atreve a borrar.
- **Activar inspección TLS sin plan**: rompe aplicaciones con certificate pinning y genera problemas legales de privacidad.
- **Creer que un firewall stateful protege la capa 7** sin IPS ni WAF.

### Para profundizar
- [NIST SP 800-41 Rev. 1: guía de firewalls](https://csrc.nist.gov/pubs/sp/800/41/r1/final)
- [Microsoft Learn: características de Azure Firewall Premium](https://learn.microsoft.com/azure/firewall/premium-features)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre un firewall stateless y uno stateful?
> **Stateless** evalúa cada paquete aislado (como una ACL); hay que permitir el tráfico de regreso a mano. **Stateful** guarda una tabla de conexiones y permite automáticamente las respuestas a sesiones iniciadas desde dentro.

> [!question]- ¿Qué agrega un firewall de nueva generación (NGFW)?
> Identificación de aplicaciones (no solo puertos), IPS, filtrado de URL, inspección de TLS, reglas por usuario o grupo e inteligencia de amenazas.

> [!question]- ¿Qué política base se recomienda?
> **Denegar por defecto** y permitir solo lo necesario (mínimo privilegio), registrando lo denegado.

