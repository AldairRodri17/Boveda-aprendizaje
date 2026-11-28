---
proyecto: Conocimiento
tipo: concepto
nivel: avanzado
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
contenido: completo
---
# Automatización de redes (Python, Netmiko y Ansible)

Parte de: [[19-Redes/09 Avanzado/_Índice Avanzado|Avanzado]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **automatización de redes** es usar código y herramientas para configurar, verificar y documentar equipos de red en lugar de hacerlo a mano, comando por comando y equipo por equipo. Como desarrollador, aquí tienes ventaja: es aplicar a la red lo que ya haces con el software (control de versiones, revisiones, pipelines, pruebas). Las herramientas de entrada más comunes son **Python con Netmiko** (conexiones SSH programadas) y **Ansible** (automatización declarativa con playbooks). El temario del CCNA incluye conceptos de automatización y programabilidad.

### Cómo funciona
**Por qué automatizar**:
- **Consistencia**: la misma plantilla en 200 switches, sin errores de dedo.
- **Velocidad**: un cambio de NTP o de un banner en toda la red en minutos.
- **Trazabilidad**: los cambios viven en Git, con revisión en un pull request y despliegue desde un pipeline de Azure DevOps.
- **Verificación**: scripts que comprueban el estado (vecinos OSPF, versiones, puertos en err-disabled) y generan reportes.

**Netmiko** es una biblioteca de Python construida sobre Paramiko (SSH) que ya entiende los prompts de cada tipo de equipo (`cisco_ios`, `cisco_nxos`, `juniper_junos`…). Te conectas, envías comandos `show` o bloques de configuración y recibes el texto. Es **imperativo**: tú dices qué comandos enviar y en qué orden.

**Ansible** es una herramienta **sin agente** (se conecta por SSH o APIs) donde describes el **estado deseado** en archivos YAML (*playbooks*) y un **inventario** de equipos. Sus módulos para Cisco (colección `cisco.ios`) saben comparar el estado actual con el deseado y solo cambian lo necesario.

**Idempotencia**: una operación es idempotente si ejecutarla una vez o diez veces deja el mismo resultado final. Un playbook idempotente que define "el NTP debe ser 10.99.0.1" no agrega líneas duplicadas si lo corres de nuevo; solo reporta "sin cambios". Un script de Netmiko que envía comandos a ciegas no es idempotente por sí mismo: tendrías que programar la comprobación.

| Aspecto | Netmiko (Python) | Ansible |
|---|---|---|
| Estilo | Imperativo (comandos) | Declarativo (estado deseado) |
| Lenguaje | Python | YAML (más plantillas Jinja2) |
| Idempotencia | La programas tú | La dan los módulos |
| Ideal para | Scripts de consulta, lógica a medida | Configuración estándar en muchos equipos |

### Ejemplo
Script de Netmiko que respalda la configuración y revisa los vecinos OSPF de varios routers. Las credenciales vienen de variables de entorno, nunca del código:

```python
import os
from datetime import date
from netmiko import ConnectHandler

equipos = ["10.99.0.1", "10.99.0.2", "10.99.0.3"]
for host in equipos:
    dispositivo = {
        "device_type": "cisco_ios",
        "host": host,
        "username": os.environ["NET_USER"],
        "password": os.environ["NET_PASS"],
    }
    with ConnectHandler(**dispositivo) as conexion:
        nombre = conexion.find_prompt().strip("#>")
        config = conexion.send_command("show running-config")
        with open(f"{nombre}-{date.today()}.cfg", "w", encoding="utf-8") as f:
            f.write(config)
        vecinos = conexion.send_command("show ip ospf neighbor", use_textfsm=True)
        print(nombre, "vecinos OSPF:", len(vecinos) if isinstance(vecinos, list) else vecinos)
        # Cambio de configuración: también es posible
        conexion.send_config_set(["ntp server 10.99.0.1"])
        conexion.save_config()
```

Este código entra a cada router por SSH, guarda la running-config en un archivo con su nombre y fecha, cuenta sus vecinos OSPF (con TextFSM convierte la salida en una lista) y agrega un servidor NTP.

Lo mismo de NTP con Ansible, de forma idempotente:

```yaml
# playbook ntp.yml
- name: Estandarizar NTP
  hosts: routers
  gather_facts: false
  tasks:
    - name: Servidor NTP de la empresa
      cisco.ios.ios_config:
        lines:
          - ntp server 10.99.0.1
        save_when: modified
```

```bash
ansible-playbook -i inventario.yml ntp.yml --check --diff   # simula y muestra diferencias
ansible-playbook -i inventario.yml ntp.yml
```

Practica sin equipos reales con los laboratorios gratuitos de Cisco DevNet o con equipos virtuales en CML.

### Cuándo usarlo y cuándo no
Automatiza tareas **repetitivas y bien entendidas**: respaldos, inventarios, plantillas base, auditorías de cumplimiento. Empieza por tareas de **solo lectura** (show) antes de cambiar configuración. No automatices algo que todavía no sabes hacer a mano: automatizarás también el error. Para equipos modernos, prefiere APIs estructuradas (ver [[NETCONF y RESTCONF]]) frente a "raspar" texto de la CLI.

### Errores comunes
- **Credenciales en el código o en el repositorio**: usa variables de entorno, Ansible Vault o Azure Key Vault.
- **Ejecutar en todos los equipos sin probar** en uno o dos primero (o sin `--check`).
- **Parsear texto de `show` con expresiones regulares frágiles**: usa TextFSM, Genie o APIs.
- **No guardar la configuración** tras el cambio.

### Para profundizar
- [Netmiko en GitHub](https://github.com/ktbyers/netmiko)
- [Colección de Ansible cisco.ios en GitHub](https://github.com/ansible-collections/cisco.ios)
- [Cisco DevNet](https://developer.cisco.com/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Por qué automatizar la configuración de red?
> Menos errores manuales, cambios repetibles en muchos equipos, configuración como código (versionada en Git) y verificación automática.

> [!question]- ¿Qué hace este código de Netmiko?
> ```python
> from netmiko import ConnectHandler
> r1 = ConnectHandler(device_type='cisco_ios', host='192.168.1.1',
>                     username='admin', password='Adm1n!')
> print(r1.send_command('show ip interface brief'))
> r1.send_config_set(['interface g0/1', 'description WAN'])
> r1.disconnect()
> ```
> Abre una sesión SSH, imprime el resumen de interfaces, aplica una descripción en G0/1 y cierra.

> [!question]- ¿Qué diferencia hay entre Netmiko y Ansible?
> **Netmiko** es una biblioteca de Python (tú escribes la lógica). **Ansible** es una herramienta declarativa sin agente: describes el estado deseado en playbooks YAML con módulos como `cisco.ios.ios_config`, y busca ser idempotente.

> [!question]- ¿Qué significa idempotente?
> Ejecutar la misma tarea varias veces deja el mismo resultado; si ya está aplicada, no cambia nada.

