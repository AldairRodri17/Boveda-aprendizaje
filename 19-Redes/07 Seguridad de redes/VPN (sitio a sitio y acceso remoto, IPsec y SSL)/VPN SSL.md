---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# VPN SSL

Parte de: [[VPN (sitio a sitio y acceso remoto, IPsec y SSL)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **VPN SSL** (más correctamente, VPN TLS, porque SSL está obsoleto) crea el túnel cifrado usando **TLS**, el mismo protocolo que protege HTTPS. Se usa principalmente para **acceso remoto** de usuarios. Su gran ventaja es que viaja normalmente sobre **TCP 443** (y opcionalmente DTLS sobre UDP 443), un puerto que casi ninguna red bloquea, así que funciona desde hoteles, aeropuertos y redes restrictivas donde IPsec (UDP 500/4500 y ESP) suele fallar.

### Cómo funciona
**Dos modalidades**:
- **Con cliente (full tunnel o split tunnel)**: el usuario instala un cliente (Cisco Secure Client, OpenVPN, FortiClient, GlobalProtect). Tras autenticarse, el cliente crea una interfaz virtual, recibe una IP interna y puede enviar **cualquier tráfico IP** por el túnel, como una VPN IPsec. Muchos clientes usan TLS para el control y **DTLS** (TLS sobre UDP) para los datos, porque TCP dentro de TCP rinde mal con pérdida de paquetes.
- **Sin cliente (clientless)**: el usuario entra con su navegador a un **portal web** del gateway, se autentica y ve enlaces a aplicaciones internas (intranet, correo web, RDP o SSH por HTML5). Solo da acceso a lo publicado en el portal, no a la red completa.

Flujo general con cliente:
1. El cliente abre una conexión TLS al gateway en el puerto 443 y valida su **certificado**.
2. El usuario se autentica (usuario y contraseña, MFA, certificado, SAML).
3. El gateway entrega la configuración: IP, DNS, rutas o reglas de split tunnel.
4. El tráfico de la interfaz virtual se encapsula en TLS/DTLS hacia el gateway.

**VPN SSL frente a IPsec para acceso remoto**:

| Criterio | VPN SSL/TLS | IPsec IKEv2 |
|---|---|---|
| Puertos | TCP/UDP 443 | UDP 500, 4500 y ESP |
| Atraviesa firewalls restrictivos | Casi siempre | A veces no |
| Cliente | Normalmente del fabricante | Nativo en muchos sistemas |
| Acceso sin cliente | Sí (portal web) | No |
| Granularidad por aplicación | Fácil en el portal | Por redes |

### Ejemplo
Un desarrollador de Itzamna viaja y necesita revisar logs en un servidor interno desde la red del hotel, que solo permite navegación web. Con IPsec la conexión no se establece (UDP 500 bloqueado). Con la VPN SSL del firewall de la oficina:
1. Abre Cisco Secure Client y se conecta a `vpn.itzamna.mx`.
2. Se autentica con SAML (Entra ID) y aprueba la notificación MFA en su iPhone.
3. Recibe una IP del pool `10.200.0.0/24` y rutas hacia `10.0.0.0/16`.
4. Accede por SSH al servidor.

Para un contratista que solo debe ver el panel de reportes web de Wee.Reporteria, se le da acceso **sin cliente** al portal con solo ese marcador, sin ninguna ruta hacia la red interna.

Verificación en el lado del usuario:

```powershell
Get-NetAdapter | Where-Object InterfaceDescription -like "*Cisco*"
Test-NetConnection vpn.itzamna.mx -Port 443
```

Comprueba con [[Wireshark (captura y análisis de tráfico)]] que el tráfico hacia el gateway se ve como TLS o DTLS (`tls || dtls`) y no se puede leer su contenido.

### Cuándo usarlo y cuándo no
Prefiere VPN SSL para acceso remoto de usuarios móviles, contratistas y equipos no administrados (en modo sin cliente). Para VPN sitio a sitio entre gateways, lo estándar sigue siendo IPsec. Recuerda que los gateways VPN SSL expuestos a internet han sido históricamente blanco de vulnerabilidades graves: mantenlos actualizados, limita la superficie del portal y usa MFA.

### Errores comunes
- **Llamarle "SSL" y usar versiones antiguas**: solo TLS 1.2 o 1.3.
- **Certificado del gateway autofirmado** que los usuarios aceptan sin validar: abre la puerta a MITM.
- **Portal sin cliente con acceso amplio** a la red.
- **Olvidar DTLS** y forzar todo por TCP: rendimiento pobre en redes con pérdida.

### Para profundizar
- [RFC 8446: TLS 1.3](https://www.rfc-editor.org/rfc/rfc8446)
- [RFC 9147: DTLS 1.3](https://www.rfc-editor.org/rfc/rfc9147)
- [NIST SP 800-46 Rev. 2: acceso remoto](https://csrc.nist.gov/pubs/sp/800/46/r2/final)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es una VPN SSL?
> Una VPN que usa **TLS** (normalmente TCP 443), igual que HTTPS, por lo que atraviesa casi cualquier firewall o red de hotel.

> [!question]- ¿Cuándo preferirías VPN SSL sobre IPsec?
> Para usuarios remotos en redes desconocidas o cuando solo necesitas acceso a aplicaciones web desde el navegador.

