---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Redes
herramienta: Cisco Packet Tracer
tags: [redes, laboratorio]
---
# Lab 1 - SSH y respaldo de configuración en Packet Tracer

Parte de: [[19-Redes/08 Cisco IOS y práctica/_Índice Cisco IOS y práctica|Cisco IOS y práctica]]

**Cubre:** [[Cisco IOS - modos y comandos básicos]] · [[Configuración inicial de un switch Cisco]] · [[Configuración inicial de un router Cisco]] · [[SSH y acceso seguro a dispositivos]] · [[Respaldo de configuraciones]] · [[FTP, SFTP y TFTP]] · [[Cisco Packet Tracer]] · [[Laboratorios de Packet Tracer (bitácora)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Dejar un router y un switch con configuración inicial segura, acceso solo por SSH y respaldo de su configuración en un servidor TFTP.

## Topología

```
PC-ADMIN ─┐
          ├─ SW1 ── G0/0 R1
TFTP-SRV ─┘
```

Red `192.168.1.0/24`: R1 `.1`, SW1 (VLAN 1) `.2`, TFTP-SRV `.10`, PC-ADMIN `.20`.

## Pasos

1. Arma la topología (router 2911, switch 2960, Server-PT y PC). En el servidor verifica que el servicio **TFTP** esté activo.
2. En R1 y SW1: hostname, `enable secret`, contraseña de consola, `service password-encryption`, `no ip domain-lookup` y un banner MOTD.
3. Da IP a G0/0 de R1 y a la VLAN 1 de SW1 (con `ip default-gateway`).
4. Configura SSH v2 en ambos con usuario local y llaves RSA de 1024 bits. Las líneas VTY solo deben aceptar SSH y cerrar la sesión a los 5 minutos sin actividad.
5. Desde PC-ADMIN (Command Prompt) entra por SSH a R1 y a SW1. Intenta también Telnet.
6. Respalda la running-config de R1 y SW1 en TFTP-SRV.
7. Cambia algo en R1 (p. ej. la descripción de G0/0), luego restaura desde el respaldo y compara.
8. Registra el laboratorio en [[Laboratorios de Packet Tracer (bitácora)]].
9. **Reto:** agrega `login block-for` en R1 y prueba con contraseñas incorrectas.

## Criterios de terminado

- [ ] `ssh -l admin 192.168.1.1` desde PC-ADMIN pide contraseña y entra a R1; lo mismo con SW1.
- [ ] `telnet 192.168.1.1` es rechazado.
- [ ] `show ip ssh` muestra **SSH Enabled - version 2.0**.
- [ ] `show running-config` no muestra contraseñas en texto claro.
- [ ] En TFTP-SRV (Services → TFTP) aparecen `R1-confg` y `SW1-confg`.
- [ ] Sé explicar qué pasa al copiar el respaldo a running-config frente a startup-config.
- [ ] Anoté el laboratorio en la bitácora.

## Solución

> [!success]- R1 (SW1 es igual con `hostname SW1`, `interface vlan 1` e `ip default-gateway 192.168.1.1`)
> ```
> enable
> configure terminal
> hostname R1
> enable secret Cl4ss!
> service password-encryption
> no ip domain-lookup
> banner motd #Acceso solo para personal autorizado#
> line console 0
>  password Cons0la
>  login
> interface g0/0
>  ip address 192.168.1.1 255.255.255.0
>  no shutdown
> ip domain-name lab.local
> username admin privilege 15 secret Adm1n!
> crypto key generate rsa general-keys modulus 1024
> ip ssh version 2
> line vty 0 4
>  transport input ssh
>  login local
>  exec-timeout 5 0
> end
> copy running-config startup-config
> ```

> [!success]- Respaldo y restauración
> ```
> R1# copy running-config tftp:
> Address or name of remote host []? 192.168.1.10
> Destination filename [R1-confg]? R1-confg
> ```
> Restaurar: `copy tftp: running-config` (se **fusiona** con lo actual: las líneas nuevas que no estén en el respaldo se quedan). Para dejarla idéntica: `copy tftp: startup-config` y `reload`.

> [!success]- Reto: bloqueo por intentos fallidos
> ```
> login block-for 120 attempts 3 within 60
> ```
> Tras 3 fallos en 60 s, el equipo rechaza nuevos inicios de sesión durante 120 s. Revisa con `show login` (si tu versión de Packet Tracer lo soporta).

## Notas personales

_Qué me costó, errores que cometí, dudas._
