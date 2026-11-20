---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Port security

Parte de: [[19-Redes/02 Switching/_Índice Switching|Switching]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Port security** es una función de los switches Cisco que **limita qué direcciones MAC y cuántas** pueden usar un puerto de acceso. Si aparece una MAC no autorizada o se supera el máximo, el switch reacciona según el modo de violación configurado. Sirve para impedir que alguien conecte un equipo no autorizado (o un switch con varios equipos) y para frenar ataques de **MAC flooding**, que llenan la tabla MAC para que el switch inunde todo como un hub.

### Cómo funciona
1. Se habilita en un puerto que debe estar en modo **access** (o trunk en algunos modelos, pero lo normal es access).
2. Se define el **máximo** de MAC permitidas (por defecto **1**).
3. Las MAC autorizadas pueden ser:
   - **Estáticas**: escritas a mano.
   - **Dinámicas**: aprendidas y olvidadas al reiniciar o caer el puerto.
   - **Sticky**: aprendidas dinámicamente y escritas en la running-config; si guardas la configuración, sobreviven al reinicio.
4. Si llega una trama de una MAC nueva cuando ya se alcanzó el máximo, ocurre una **violación**.

**Modos de violación**:

| Modo | ¿Descarta tráfico no autorizado? | ¿Registra (syslog/SNMP) y cuenta? | ¿Apaga el puerto? |
|---|---|---|---|
| **protect** | Sí | No | No |
| **restrict** | Sí | Sí | No |
| **shutdown** (predeterminado) | Sí | Sí | Sí, pasa a **err-disabled** |

**Recuperar un puerto err-disabled**: primero elimina la causa (desconecta el equipo no autorizado o ajusta la configuración), luego `shutdown` y `no shutdown` en la interfaz. También se puede automatizar con `errdisable recovery cause psecure-violation` y un intervalo, aunque si la causa persiste el puerto volverá a caer.

### Ejemplo
Puerto Fa0/8 del escritorio de un desarrollador, que puede tener su laptop y una dockstation con MAC propia (máximo 2), aprendidas con sticky y en modo restrict para no tumbar el puerto, pero sí registrar:

```text
SW1(config)# interface fa0/8
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan 10
SW1(config-if)# switchport port-security
SW1(config-if)# switchport port-security maximum 2
SW1(config-if)# switchport port-security mac-address sticky
SW1(config-if)# switchport port-security violation restrict
SW1(config-if)# end
SW1# show port-security interface fa0/8
Port Security              : Enabled
Port Status                : Secure-up
Violation Mode             : Restrict
Maximum MAC Addresses      : 2
Total MAC Addresses        : 2
Sticky MAC Addresses       : 2
Security Violation Count   : 0
SW1# show port-security address
SW1# show interfaces status err-disabled
```

Para un puerto en modo shutdown que se cayó:

```text
SW1(config)# interface fa0/3
SW1(config-if)# shutdown
SW1(config-if)# no shutdown
```

Recuerda que muchos equipos (iPhone, Windows) usan MAC aleatoria en Wi-Fi, pero en cable normalmente usan su MAC real; aun así, una VM con bridge en la laptop agrega otra MAC y puede disparar la violación.

### Cuándo usarlo y cuándo no
Úsalo en puertos de acceso en áreas físicamente expuestas (salas de juntas, recepción) y como defensa contra MAC flooding. No lo pongas en uplinks ni en puertos hacia hipervisores o APs con muchas MAC sin ajustar el máximo. Para un control de acceso real por identidad, la solución moderna es **802.1X** con un servidor RADIUS; port security es una medida básica complementaria.

### Errores comunes
- **Olvidar `switchport mode access`**: IOS rechaza el comando en un puerto dinámico.
- **No guardar la configuración** con sticky: tras reiniciar se pierden las MAC aprendidas.
- **Máximo de 1 en un puerto con teléfono IP y PC**: el teléfono también tiene MAC; considera al menos 2 (o 3 según el modelo).
- **Hacer `no shutdown` sin quitar la causa**: el puerto vuelve a err-disabled.

### Para profundizar
- [Cisco Catalyst 2960: guías de configuración (Port Security)](https://www.cisco.com/c/en/us/support/switches/catalyst-2960-series-switches/products-installation-and-configuration-guides-list.html)
- [IEEE 802.1X (control de acceso por puerto), IEEE 802.1 Working Group](https://www.ieee802.org/1/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Para qué sirve port security?
> Limita qué y cuántas MAC pueden usar un puerto de acceso; frena conexiones no autorizadas y ataques de inundación de la tabla MAC.

> [!question]- ¿Qué modos de violación existen y cuál es el predeterminado?
> **shutdown** (predeterminado: puerto a *err-disabled*), **restrict** (descarta, registra y cuenta) y **protect** (descarta sin avisar).

> [!question]- Configura un puerto con máximo 2 MAC aprendidas con sticky y modo restrict.
> ```
> interface fa0/3
>  switchport mode access
>  switchport port-security
>  switchport port-security maximum 2
>  switchport port-security mac-address sticky
>  switchport port-security violation restrict
> ```

> [!question]- Un puerto quedó en err-disabled por port security. ¿Cómo lo recuperas?
> Quita la causa y ejecuta `shutdown` y luego `no shutdown` en la interfaz (o configura `errdisable recovery cause psecure-violation`). Revisa con `show port-security interface fa0/3`.

## Práctica

- [[Lab 2 - STP y EtherChannel en Packet Tracer]]

