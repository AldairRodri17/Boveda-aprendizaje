---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Laboratorios y práctica
herramienta: Docker Desktop, Docker Compose
tags: [laboratorios-practica, laboratorio]
contenido: completo
---
# Lab 1 - Laboratorio propio con Docker Compose y red aislada

Parte de: [[16-Laboratorios-Practica/01 Laboratorios de seguridad/_Índice Laboratorios de seguridad|Laboratorios de seguridad]]

**Cubre:** [[Cómo montar un laboratorio propio con Docker y red aislada]] · [[OWASP Juice Shop]] · [[OWASP WebGoat]] · [[DVWA]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

> [!warning] Ética
> Las apps de este laboratorio son vulnerables a propósito. Mantenlas solo en tu equipo y no las expongas a la red. Ver [[Marco legal y autorización por escrito]].

## Objetivo
Levantar con Docker Compose un laboratorio con OWASP Juice Shop y DVWA (o WebGoat) en una red interna sin salida a internet, accesible solo desde tu equipo, y comprobar el aislamiento.

## Pasos
1. Instala Docker Desktop y verifica `docker compose version`.
2. Crea una carpeta `lab-seguridad` con un `docker-compose.yml` que defina:
   - una red `interna` con `internal: true`;
   - una red `frontal` normal;
   - Juice Shop conectado a ambas redes y con el puerto publicado solo en `127.0.0.1`;
   - una segunda app vulnerable (WebGoat o DVWA) igual;
   - un contenedor `atacante` (`alpine`, que ya trae `wget`) conectado **solo** a la red `interna`.
3. Levanta todo con `docker compose up -d`.
4. Desde tu navegador abre las apps en `localhost`.
5. Entra al contenedor `atacante` y comprueba que **sí** llega a Juice Shop por nombre de servicio y que **no** llega a internet.
6. Desde otro equipo de tu red (o el celular en la misma Wi-Fi) intenta abrir `http://<IP-de-tu-PC>:3000`: no debe responder.
7. Apaga y borra todo con `docker compose down` al terminar.

## Criterios de terminado
- [ ] `docker compose ps` muestra los servicios en ejecución.
- [ ] Juice Shop abre en `http://localhost:3000` desde tu PC.
- [ ] Desde `atacante`, `wget -qO- http://juice-shop:3000` devuelve HTML.
- [ ] Desde `atacante`, una petición a un sitio de internet falla (sin resolución DNS o sin ruta).
- [ ] Desde otro dispositivo de la red, el puerto 3000 de tu PC no responde.
- [ ] `docker compose down` deja sin contenedores ni redes del laboratorio.

## Solución

> [!success]- docker-compose.yml
> ```yaml
> services:
>   juice-shop:
>     image: bkimminich/juice-shop
>     ports:
>       - "127.0.0.1:3000:3000"
>     networks: [frontal, interna]
>
>   webgoat:
>     image: webgoat/webgoat
>     ports:
>       - "127.0.0.1:8080:8080"
>     networks: [frontal, interna]
>
>   atacante:
>     image: alpine:3
>     command: ["sleep", "infinity"]
>     networks: [interna]
>
> networks:
>   frontal: {}
>   interna:
>     internal: true
> ```
> Como `atacante` solo está en la red interna, no puede instalar paquetes; por eso se usa `wget`, que ya viene en Alpine (BusyBox).

> [!success]- Comprobaciones
> ```powershell
> docker compose up -d
> docker compose ps
> docker compose exec atacante wget -qO- http://juice-shop:3000 | Select-Object -First 3
> docker compose exec atacante wget -qO- -T 5 https://example.com   # debe fallar
> docker compose down
> ```

> [!success]- Por qué Juice Shop sí sale a internet y atacante no
> Juice Shop también está en la red `frontal` para que Docker pueda publicar su puerto en tu equipo; esa red tiene salida. Si quieres que las apps tampoco salgan, deja solo la red interna y accede a ellas con un proxy inverso en ambas redes, o acepta la salida (las apps no inician conexiones por sí solas). Lo importante es que los puertos estén publicados solo en `127.0.0.1`.

## Notas personales

_Qué me costó, errores que cometí, dudas._
