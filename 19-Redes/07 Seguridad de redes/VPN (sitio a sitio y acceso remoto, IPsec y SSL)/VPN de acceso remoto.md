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
# VPN de acceso remoto

Parte de: [[VPN (sitio a sitio y acceso remoto, IPsec y SSL)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **VPN de acceso remoto** conecta a **un usuario o dispositivo individual** con la red de la organización desde cualquier lugar con internet. A diferencia de la sitio a sitio, aquí el túnel lo inicia un **cliente de software** en la laptop o el teléfono del usuario, que recibe una IP de la red interna (o de un pool de la VPN) y accede a los recursos como si estuviera en la oficina.

### Cómo funciona
1. El usuario abre el cliente VPN y se conecta a un **concentrador** o gateway (firewall, router o servicio en la nube).
2. Se **autentica**.
3. El gateway le asigna una IP del pool VPN, le entrega DNS internos y las rutas que debe enviar por el túnel.
4. El tráfico hacia la red interna viaja cifrado; el resto depende del modo de túnel.

**Opciones tecnológicas**:
- **VPN SSL/TLS** con cliente (Cisco Secure Client, antes AnyConnect; OpenVPN; los de Fortinet o Palo Alto): viaja sobre TCP/UDP 443, pasa por casi cualquier red (ver [[VPN SSL]]).
- **VPN SSL sin cliente** (clientless): acceso por navegador a aplicaciones web publicadas en un portal.
- **IPsec IKEv2**: con clientes nativos de Windows, macOS, iOS y Android.
- **WireGuard**: protocolo moderno y sencillo, muy eficiente.
- **Azure VPN Gateway punto a sitio**: IKEv2, OpenVPN o SSTP, con autenticación por certificados o con Entra ID.

**Split tunneling**:
- **Túnel completo** (full tunnel): **todo** el tráfico del equipo, incluido el de internet, pasa por la VPN y sale por la empresa. Más control e inspección, más carga en el concentrador y más latencia para el usuario.
- **Túnel dividido** (split tunnel): solo el tráfico hacia las redes corporativas va por la VPN; internet sale directo por la conexión del usuario. Menos carga y mejor experiencia (Teams, YouTube no pasan por la oficina), pero el equipo queda expuesto a internet mientras accede a la red interna; requiere buena protección del endpoint.

**Autenticación recomendada**:
- **MFA** siempre (contraseña más segundo factor, idealmente resistente a phishing como FIDO2 o notificación con número).
- **Certificados de dispositivo** para asegurar que solo equipos administrados se conectan.
- Integración con el directorio (RADIUS, SAML con Entra ID) para dar de baja accesos en un solo lugar.
- **Verificación de postura**: sistema actualizado, antivirus activo, disco cifrado.
Evita contraseñas solas o llaves compartidas para todos los usuarios.

### Ejemplo
Política para los desarrolladores de Itzamna que trabajan desde casa:
- VPN SSL con Cisco Secure Client contra el firewall de la oficina, autenticación SAML con Entra ID y MFA, y certificado de dispositivo.
- **Split tunnel** que solo envía por la VPN `10.0.0.0/16` (oficina) y `10.50.0.0/15` (VNets de Azure).
- DNS interno para resolver `*.itzamna.local`.
- Grupo de VPN "Desarrollo" que solo permite llegar a los servidores de desarrollo, no a Finanzas.

Diagnóstico desde la laptop con Windows al estar conectado:

```powershell
Get-NetIPConfiguration                 # interfaz de la VPN con su IP y DNS
route print -4                         # rutas que mandan 10.0.0.0/16 por la VPN
Resolve-DnsName api.itzamna.local
Test-NetConnection 10.50.1.10 -Port 443
```

### Cuándo usarlo y cuándo no
Úsala cuando los empleados necesiten acceso a recursos internos que no están publicados de forma segura. Si solo necesitan aplicaciones web o SaaS, evalúa publicarlas con un proxy de identidad o un **ZTNA**, que da acceso por aplicación y aplica Zero Trust (ver [[Zero Trust en redes]]). Para proveedores externos, nunca les des la misma VPN que a los empleados: crea perfiles con acceso mínimo.

### Errores comunes
- **Sin MFA**: las credenciales robadas son la principal vía de entrada en incidentes.
- **Acceso a toda la red** para todos los usuarios VPN.
- **Pool de VPN traslapado** con redes caseras comunes (192.168.0.0/24 y 192.168.1.0/24).
- **No actualizar el concentrador VPN**: estos equipos expuestos a internet son blanco frecuente de vulnerabilidades críticas.

### Para profundizar
- [NIST SP 800-46 Rev. 2: seguridad en teletrabajo y acceso remoto](https://csrc.nist.gov/pubs/sp/800/46/r2/final)
- [Microsoft Learn: Azure VPN Gateway punto a sitio](https://learn.microsoft.com/azure/vpn-gateway/point-to-site-about)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué opciones hay de VPN de acceso remoto?
> **Con cliente** (software instalado, túnel completo de red) o **sin cliente** (portal web en el navegador con acceso a aplicaciones publicadas).

> [!question]- ¿Qué es split tunneling?
> Solo el tráfico hacia la red corporativa va por la VPN; el resto sale directo a Internet. Ahorra ancho de banda, pero el equipo queda menos controlado.

> [!question]- ¿Qué autenticación conviene?
> Usuario con **MFA** o certificados, integrada con el directorio (RADIUS/LDAP/SAML).

