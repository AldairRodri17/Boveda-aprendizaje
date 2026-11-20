---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Cisco IOS - modos y comandos básicos

Parte de: [[19-Redes/08 Cisco IOS y práctica/_Índice Cisco IOS y práctica|Cisco IOS y práctica]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Cisco IOS** (y sus variantes IOS XE e IOS XR) es el sistema operativo de los routers y switches de Cisco. Se administra principalmente por **línea de comandos (CLI)** organizada en **modos jerárquicos**: cada modo permite cierto tipo de comandos y se reconoce por su *prompt*. Dominar la navegación entre modos, la ayuda y los atajos es la base de todo lo demás en el CCNA y en [[Cisco Packet Tracer]].

### Cómo funciona
**Modos principales**:

| Modo | Prompt | Cómo entrar | Para qué |
|---|---|---|---|
| EXEC de usuario | `R1>` | Al conectarte | Comandos básicos de consulta (`ping`, `show` limitados) |
| EXEC privilegiado | `R1#` | `enable` | Todos los `show`, `debug`, `copy`, `reload` |
| Configuración global | `R1(config)#` | `configure terminal` | Cambios que afectan a todo el equipo |
| Interfaz | `R1(config-if)#` | `interface g0/1` | IP, descripción, `shutdown` |
| Subinterfaz | `R1(config-subif)#` | `interface g0/0.10` | Router on a stick |
| Línea | `R1(config-line)#` | `line console 0`, `line vty 0 15` | Acceso por consola o remoto |
| Router | `R1(config-router)#` | `router ospf 1` | Protocolos de enrutamiento |
| VLAN | `SW1(config-vlan)#` | `vlan 10` | Nombrar VLAN |

Para retroceder: `exit` sube un nivel; `end` o **Ctrl+Z** regresan directo al EXEC privilegiado.

**running-config frente a startup-config**:
- **running-config**: la configuración **activa en RAM**. Todo cambio que escribes se aplica al instante aquí.
- **startup-config**: la guardada en **NVRAM**; es la que se carga al reiniciar.
- Si no guardas con `copy running-config startup-config` (o `write memory`), los cambios se pierden al reiniciar. Útil como "deshacer" en un laboratorio y peligroso en producción.

**Ayuda y atajos**:
- `?` muestra los comandos o parámetros posibles en ese punto; `sh?` lista los que empiezan con "sh".
- **Tab** completa la palabra.
- Las **abreviaturas** funcionan si no son ambiguas: `conf t`, `sh ip int br`, `int g0/1`, `no shut`.
- Flecha arriba o **Ctrl+P** repite comandos anteriores; **Ctrl+A** y **Ctrl+E** van al inicio y fin de la línea.
- **Ctrl+Shift+6** interrumpe un ping, traceroute o búsqueda DNS que se quedó colgada.
- `do` antepuesto ejecuta un comando EXEC sin salir del modo de configuración: `do show ip interface brief`.

**Filtrar la salida** con tuberías: `| include`, `| exclude`, `| begin`, `| section`.

### Ejemplo
De `R1>` a configurar una interfaz y verificar sin salir:

```text
R1> enable
R1# configure terminal
R1(config)# interface g0/1
R1(config-if)# description Enlace a SW1
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# do show ip interface brief | include 0/1
GigabitEthernet0/1   192.168.1.1   YES manual up   up
R1(config-if)# end
R1# show running-config | section router ospf
R1# show running-config interface g0/1
R1# copy running-config startup-config
```

Borrar la configuración guardada y empezar de cero (en un switch, además, la base de VLAN):

```text
R1# write erase              ! equivalente: erase startup-config
R1# reload
SW1# delete flash:vlan.dat
```

Al reiniciar, IOS preguntará si quieres guardar la configuración modificada: responde **no** si quieres empezar limpio, o volverás a escribir la running-config actual.

Comandos `show` que usarás todo el tiempo: `show version`, `show running-config`, `show ip interface brief`, `show interfaces`, `show ip route`, `show vlan brief`, `show mac address-table`, `show cdp neighbors`, `show logging`.

### Cuándo usarlo y cuándo no
La CLI es la herramienta principal del CCNA y de la operación diaria. En redes grandes se complementa con automatización (Python, Ansible, APIs) y controladores (ver [[Automatización de redes (Python, Netmiko y Ansible)]]), pero siempre necesitarás la CLI para diagnosticar.

### Errores comunes
- **No guardar** la configuración.
- **Escribir comandos en el modo equivocado** (`ip address` en configuración global).
- **Olvidar `no ip domain-lookup`** en el laboratorio y esperar a que el router "resuelva" un comando mal escrito.
- **Usar `debug` intensivo en producción**: puede saturar la CPU.

### Para profundizar
- [Cisco Packet Tracer en Networking Academy](https://www.netacad.com/cisco-packet-tracer)
- [Cisco: examen 200-301 CCNA](https://www.cisco.com/site/us/en/learn/training-certifications/exams/ccna.html)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- ¿En qué modo estás y cómo llegas a configurar G0/1 desde `R1>`?
> Estás en EXEC usuario. `enable` → `configure terminal` → `interface g0/1`.

> [!question]- Escribe los comandos para ver solo la sección de OSPF de la configuración.
> `show running-config | section router ospf`

> [!question]- Borra la configuración guardada y reinicia el equipo.
> `erase startup-config` y luego `reload` (contesta *no* si pregunta si guardar).

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- Nombra los modos de IOS y su prompt.
> EXEC usuario `>` · EXEC privilegiado `#` (con `enable`) · Configuración global `(config)#` (con `configure terminal`) · Interfaz `(config-if)#` · Línea `(config-line)#`.

> [!question]- ¿Qué diferencia hay entre running-config y startup-config?
> **running-config** está en RAM (lo que funciona ahora). **startup-config** está en NVRAM y se carga al arrancar. Se guarda con `copy running-config startup-config`.

> [!question]- ¿Cómo ejecutas un `show` sin salir del modo de configuración?
> Con `do`: `do show ip interface brief`.

> [!question]- ¿Qué atajos ahorran tiempo?
> `?` (ayuda), Tab (completar), abreviaturas (`conf t`, `sh run`), Ctrl+Z (volver a EXEC privilegiado) y `show run | include`/`section` para filtrar.

## Práctica

- [[Lab 1 - SSH y respaldo de configuración en Packet Tracer]]

