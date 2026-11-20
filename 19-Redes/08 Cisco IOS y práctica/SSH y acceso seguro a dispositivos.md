---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# SSH y acceso seguro a dispositivos

Parte de: [[19-Redes/08 Cisco IOS y práctica/_Índice Cisco IOS y práctica|Cisco IOS y práctica]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**SSH** (Secure Shell, TCP 22) es el protocolo para administrar equipos de red de forma remota con **cifrado** y **autenticación**. Reemplaza a **Telnet** (TCP 23), que envía todo en texto claro, incluidos usuario y contraseña: cualquiera que capture el tráfico (por ejemplo, con un ataque ARP spoofing) los lee. En Cisco IOS se recomienda **SSH versión 2** y deshabilitar Telnet por completo.

### Cómo funciona
Requisitos para que IOS acepte SSH:
1. Un **hostname** distinto del predeterminado (`Router` o `Switch`).
2. Un **nombre de dominio** (`ip domain-name`).
3. Un **par de llaves RSA** generado con `crypto key generate rsa`; el nombre de la llave se forma con hostname y dominio. Usa al menos **2048 bits** (las versiones de SSH v2 requieren mínimo 768, pero 2048 es la práctica actual).
4. Un **método de autenticación**: usuarios locales (`username ... secret ...`) o AAA con RADIUS/TACACS+.
5. Las **líneas VTY** con `login local` (o AAA) y `transport input ssh`.

Por eso `crypto key generate rsa` falla en un equipo recién configurado: si no cambiaste el hostname o no definiste el dominio, IOS responde con un error del tipo "Please define a hostname other than Router" o "Please define a domain-name first".

### Ejemplo
SSH v2 con usuario local en R1, solo desde la red de gestión:

```text
R1(config)# hostname R1
R1(config)# ip domain-name itzamna.local
R1(config)# crypto key generate rsa modulus 2048
R1(config)# ip ssh version 2
R1(config)# ip ssh time-out 60
R1(config)# ip ssh authentication-retries 3
R1(config)# username admin privilege 15 secret <ClaveAdmin>
R1(config)# ip access-list standard GESTION-VTY
R1(config-std-nacl)# permit 10.99.0.0 0.0.0.255
R1(config-std-nacl)# exit
R1(config)# line vty 0 15
R1(config-line)# login local
R1(config-line)# transport input ssh
R1(config-line)# access-class GESTION-VTY in
R1(config-line)# exec-timeout 10 0
R1(config-line)# end
R1# show ip ssh
SSH Enabled - version 2.0
R1# show ssh
```

Conexión desde tu PC (Windows 10/11 trae el cliente OpenSSH):

```bash
ssh admin@10.99.0.1
# En equipos antiguos puede requerir habilitar algoritmos heredados en el cliente
```

En [[Cisco Packet Tracer]], desde una PC usa Desktop → Command Prompt: `ssh -l admin 10.99.0.1`.

**Otras medidas de endurecimiento**:
- `enable secret` (hash fuerte, en versiones recientes con algoritmos tipo 8 o 9) en lugar de `enable password`.
- `login block-for 120 attempts 3 within 60`: bloquea logins tras varios fallos (protección contra fuerza bruta).
- `security passwords min-length 12`.
- **AAA centralizado** (RADIUS o TACACS+) para tener usuarios individuales, contabilidad de comandos y bajas centralizadas.
- **ACL en VTY** para que solo la red de gestión o un bastión pueda conectarse.
- Banner legal, `exec-timeout`, apagar servicios innecesarios (`no ip http server`, `no cdp run` en interfaces hacia internet).
- **Gestión fuera de banda** y registro de los accesos en syslog.

Las contraseñas reales deben vivir en un gestor de secretos, no en documentos ni en los respaldos compartidos.

### Cuándo usarlo y cuándo no
Siempre SSH v2 para administración remota. Telnet solo en un laboratorio aislado si el equipo no soporta SSH (cada vez más raro). Para automatización, las herramientas como Netmiko y Ansible se conectan por SSH, o mejor por APIs (NETCONF, RESTCONF) que también van cifradas.

### Errores comunes
- **Olvidar el dominio** antes de generar las llaves.
- **Dejar `transport input all`** o `telnet ssh`: Telnet sigue abierto.
- **Configurar solo `line vty 0 4`** cuando el equipo tiene 16 líneas: las 5 a 15 quedan con la configuración por defecto.
- **Usar `login`** en lugar de `login local` sin contraseña en la línea: nadie puede entrar (o entra cualquiera con la clave de línea).

### Para profundizar
- [Cisco: Configure Secure Shell on Routers and Switches](https://www.cisco.com/c/en/us/support/docs/security-vpn/secure-shell-ssh/4145-ssh.html)
- [Cisco Guide to Harden Cisco IOS Devices](https://www.cisco.com/c/en/us/support/docs/ip/access-lists/13608-21.html)
- [RFC 4253: SSH Transport Layer Protocol](https://www.rfc-editor.org/rfc/rfc4253)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Por qué usar SSH y no Telnet?
> Telnet envía todo (incluida la contraseña) en texto claro; SSH cifra la sesión.

> [!question]- Lista los comandos para habilitar SSH v2 con usuario local.
> ```
> hostname R1
> ip domain-name lab.local
> username admin privilege 15 secret Adm1n!
> crypto key generate rsa modulus 1024
> ip ssh version 2
> line vty 0 4
>  transport input ssh
>  login local
> ```
> SSH v2 necesita llaves de al menos 768 bits; usa 1024 o más (en equipos reales, 2048).

> [!question]- ¿Por qué falla `crypto key generate rsa` en un equipo recién configurado?
> Falta el **hostname** distinto al predeterminado o el **`ip domain-name`**; la llave se nombra con ambos.

> [!question]- ¿Qué otras medidas endurecen el acceso?
> `exec-timeout 5 0` en las líneas, `login block-for` contra fuerza bruta, `access-class` en VTY para limitar orígenes, `service password-encryption` y banner de aviso legal.

## Práctica

- [[Lab 1 - SSH y respaldo de configuración en Packet Tracer]]

