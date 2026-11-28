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
# Zero Trust en redes

Parte de: [[19-Redes/07 Seguridad de redes/_Índice Seguridad de redes|Seguridad de redes]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Zero Trust** (confianza cero) es un **modelo de seguridad**, no un producto, que se resume en **"nunca confíes, siempre verifica"**. Abandona la idea de que todo lo que está dentro de la red corporativa es confiable y lo de afuera no. Cada acceso a un recurso se autentica, se autoriza y se evalúa en el momento, según la identidad, el estado del dispositivo y el contexto, sin importar desde qué red venga. El NIST lo describe en la publicación SP 800-207.

### Cómo funciona
Principios:
1. **Verificar explícitamente** cada acceso: identidad fuerte (MFA), estado del dispositivo (actualizado, cifrado, administrado), ubicación, riesgo.
2. **Mínimo privilegio**: dar solo el acceso necesario, por el tiempo necesario (acceso just-in-time y just-enough).
3. **Asumir que ya hubo una brecha**: segmentar para limitar el movimiento lateral, cifrar todo, registrar y analizar.

Componentes de la arquitectura (según NIST): un **punto de decisión de políticas** (PDP), que evalúa la solicitud, y un **punto de aplicación de políticas** (PEP), que permite o bloquea el acceso (un proxy, un gateway, un agente en el equipo).

**Prácticas de red** que lo implementan:
- **Microsegmentación**: reglas por aplicación o carga de trabajo, no solo por VLAN. Un servidor de la API solo habla con su base de datos y con nada más.
- **Control de acceso a la red (NAC) con 802.1X**: autenticar cada dispositivo al conectarse, cableado o inalámbrico, y asignar VLAN o política según identidad y postura.
- **ZTNA** (Zero Trust Network Access): sustituye la VPN tradicional que daba acceso a toda la red por accesos por aplicación, mediados por un proxy de identidad.
- **Cifrado en todos los saltos**: TLS entre servicios (mTLS en malla de servicios), aunque sea tráfico interno.
- **Gestión segura de la administración**: bastiones, cuentas con privilegios temporales, sin acceso directo desde la LAN de usuarios a la gestión.
- **Visibilidad**: registros de flujos y de autenticación centralizados (SIEM) para detectar comportamientos anómalos.

**Zero Trust no es un producto**: los fabricantes venden piezas (ZTNA, NAC, microsegmentación, SASE), pero el modelo requiere procesos, identidad bien gobernada, inventario de dispositivos y políticas. Se adopta gradualmente.

### Ejemplo
Cómo se aplica al ecosistema de tus proyectos:
- **Identidad**: Xolotl.Hecate emite tokens JWT firmados con RS256, con PKCE para los clientes públicos; cada llamada a la API de Itzamna.Dionisio valida el token, la audiencia y los permisos (scopes) antes de ejecutar nada. Eso es "verificar explícitamente" en la capa de aplicación.
- **Mínimo privilegio**: la API usa una identidad administrada en Azure con permisos solo sobre su base de datos y su Key Vault; los secretos nunca están en el repositorio.
- **Red**: la base de datos solo acepta conexiones desde la subnet de la API por Private Endpoint y un NSG; no tiene IP pública.
- **Usuarios**: el acceso de administradores pasa por Azure Bastion o acceso just-in-time, con MFA y acceso condicional.
- **Supervisión**: logs de NSG flow, Entra ID y la aplicación en un mismo Log Analytics.

En la red del campus, 802.1X con un servidor RADIUS:

```text
SW1(config)# aaa new-model
SW1(config)# aaa authentication dot1x default group radius
SW1(config)# dot1x system-auth-control
SW1(config)# interface g1/0/5
SW1(config-if)# switchport mode access
SW1(config-if)# authentication port-control auto
SW1(config-if)# dot1x pae authenticator
```

### Cuándo usarlo y cuándo no
Es la dirección de la industria para cualquier organización con trabajo remoto, nube y aplicaciones SaaS. Empieza por lo que más reduce riesgo: MFA en todas las cuentas, inventario de dispositivos, segmentación de servidores críticos y retiro de accesos amplios por VPN. No es un proyecto de "comprar y listo"; tampoco elimina la necesidad de firewalls, que pasan a ser uno de los puntos de aplicación.

### Errores comunes
- **Comprar un producto con la etiqueta Zero Trust** y no cambiar los procesos.
- **Seguir confiando en la LAN interna** ("está detrás del firewall").
- **MFA solo para algunos usuarios** o con excepciones permanentes.
- **No tener inventario** de dispositivos ni de aplicaciones: no puedes aplicar políticas a lo que no conoces.

### Para profundizar
- [NIST SP 800-207: Zero Trust Architecture](https://csrc.nist.gov/pubs/sp/800/207/final)
- [Microsoft Learn: guía de Zero Trust](https://learn.microsoft.com/security/zero-trust/zero-trust-overview)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué principio resume Zero Trust?
> **Nunca confiar, siempre verificar**: estar dentro de la red no da confianza; cada acceso se autentica y autoriza según identidad, dispositivo y contexto. Referencia: NIST SP 800-207.

> [!question]- ¿Qué prácticas de red lo implementan?
> Microsegmentación, mínimo privilegio, MFA, verificación de la postura del dispositivo, acceso por aplicación (ZTNA) en lugar de VPN de red completa y monitoreo continuo.

> [!question]- ¿Zero Trust es un producto?
> No: es un modelo de arquitectura que se adopta por etapas con varios controles.

