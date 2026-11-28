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
# Redundancia de gateway (HSRP y VRRP)

Parte de: [[19-Redes/05 Routing/_Índice Routing|Routing]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Los equipos finales tienen configurada **una sola puerta de enlace**. Si ese router o switch capa 3 falla, todos los equipos de la VLAN se quedan sin salida aunque haya otro router disponible. Los **protocolos de redundancia de primer salto** (FHRP) resuelven esto: dos o más routers comparten una **IP virtual** (y una MAC virtual) que los hosts usan como gateway; uno atiende y otro está listo para tomar el lugar en segundos. Los principales son **HSRP** (de Cisco), **VRRP** (estándar abierto) y **GLBP** (Cisco, con balanceo).

### Cómo funciona
1. Se configura en cada router una interfaz en la VLAN con su IP real (por ejemplo `.2` y `.3`) y la misma **IP virtual** (por ejemplo `.1`) en un **grupo**.
2. Los routers intercambian mensajes hello por multicast y eligen quién es el **activo** (HSRP) o **maestro** (VRRP) según la **prioridad** (mayor gana; por defecto **100** en ambos) y, si empatan, la IP más alta.
3. El activo responde ARP por la IP virtual con la **MAC virtual**. Los hosts usan esa MAC sin saber qué router está detrás.
4. Si el activo deja de enviar hellos (falla), el de respaldo asume la IP y la MAC virtuales y envía un **ARP gratuito** para que los switches actualicen sus tablas MAC. Los hosts no notan el cambio, no tienen que reconfigurar nada.
5. **Preempt**: si el router de mayor prioridad regresa, ¿recupera el rol? En HSRP **no**, salvo que se configure `preempt`; en VRRP **sí** por defecto.
6. Con **tracking** se baja la prioridad del activo si cae su enlace hacia internet, para que el otro tome el control aunque el primero siga encendido.

| Característica | HSRP | VRRP |
|---|---|---|
| Estándar | Propietario de Cisco | IETF (RFC 5798, versión 3) |
| Roles | Active / Standby | Master / Backup |
| Multicast | 224.0.0.2 (v1), 224.0.0.102 (v2) | 224.0.0.18 |
| MAC virtual (IPv4) | 0000.0c07.acXX (v1), 0000.0c9f.fXXX (v2) | 0000.5e00.01XX |
| Prioridad por defecto | 100 | 100 |
| Preempt por defecto | No | Sí |
| IP virtual igual a IP real | No | Sí, se permite (el dueño tiene prioridad 255) |

### Ejemplo
Dos switches núcleo (o routers) para la VLAN 10 `192.168.10.0/24`. Los hosts usan `192.168.10.1` como gateway:

```text
CORE1(config)# interface vlan 10
CORE1(config-if)# ip address 192.168.10.2 255.255.255.0
CORE1(config-if)# standby version 2
CORE1(config-if)# standby 10 ip 192.168.10.1
CORE1(config-if)# standby 10 priority 110
CORE1(config-if)# standby 10 preempt
CORE1(config-if)# standby 10 track 1 decrement 20

CORE2(config)# interface vlan 10
CORE2(config-if)# ip address 192.168.10.3 255.255.255.0
CORE2(config-if)# standby version 2
CORE2(config-if)# standby 10 ip 192.168.10.1
CORE2(config-if)# standby 10 preempt

CORE1# show standby brief
Interface  Grp  Pri P State   Active          Standby         Virtual IP
Vl10       10   110 P Active  local           192.168.10.3    192.168.10.1
```

El `track 1` se definiría antes con `track 1 interface g1/0/24 line-protocol` (el enlace hacia internet). Si cae, la prioridad de CORE1 baja a 90 y CORE2 (100) toma el rol activo.

Equivalente en VRRP:

```text
R1(config-if)# vrrp 10 ip 192.168.10.1
R1(config-if)# vrrp 10 priority 110
R1# show vrrp brief
```

Una práctica común es alinear el **activo de HSRP con el root bridge de STP** de cada VLAN para que el tráfico no haga recorridos innecesarios.

### Cuándo usarlo y cuándo no
Úsalo en toda VLAN donde haya dos equipos capaces de ser gateway, típicamente en la capa de distribución o núcleo. HSRP si tu red es solo Cisco y quieres lo que más documentación tiene en ese mundo; VRRP si mezclas fabricantes. Para repartir carga entre ambos routers, configura grupos distintos activos en cada uno (por VLAN) o usa GLBP.

### Errores comunes
- **Gateway de los hosts apuntando a la IP real** de un router en lugar de la virtual.
- **Olvidar `preempt`** en HSRP y no entender por qué el router principal no recupera el rol.
- **Número de grupo o versión distinta** en cada router: ambos se declaran activos.
- **No usar tracking**: el activo sigue activo aunque haya perdido la salida a internet.

### Para profundizar
- [RFC 5798: VRRP versión 3](https://www.rfc-editor.org/rfc/rfc5798)
- [RFC 2281: Cisco Hot Standby Router Protocol (HSRP)](https://www.rfc-editor.org/rfc/rfc2281)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué problema resuelven HSRP y VRRP?
> Dan una **IP virtual** de puerta de enlace compartida por dos o más routers; si el activo falla, otro toma el papel sin cambiar nada en los hosts.

> [!question]- ¿Qué diferencias hay entre HSRP y VRRP?
> HSRP es de Cisco (activo/standby, preempt apagado por defecto). VRRP es estándar (master/backup, preempt encendido por defecto, la IP virtual puede ser la real del router).

> [!question]- Configura R1 como activo de HSRP grupo 1 con IP virtual 192.168.1.254.
> ```
> interface g0/0
>  standby 1 ip 192.168.1.254
>  standby 1 priority 110
>  standby 1 preempt
> ```
> Verifica con `show standby brief`. La prioridad por defecto es 100.

