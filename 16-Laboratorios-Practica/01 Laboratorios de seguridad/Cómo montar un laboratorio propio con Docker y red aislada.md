---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Laboratorios y práctica
tags: [laboratorios-practica]
contenido: completo
---
# Cómo montar un laboratorio propio con Docker y red aislada

Parte de: [[16-Laboratorios-Practica/01 Laboratorios de seguridad/_Índice Laboratorios de seguridad|Laboratorios de seguridad]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Es la práctica de levantar tus propias aplicaciones vulnerables (como [[OWASP Juice Shop]], [[DVWA]] o [[OWASP WebGoat]]) y tus herramientas en **contenedores Docker**, dentro de una **red aislada**, para practicar seguridad sin depender de una plataforma en línea y, sobre todo, **sin exponer nada a internet**. Montar el laboratorio es, en sí, un ejercicio que enseña Docker, redes y buenas prácticas.

### Por qué montar un laboratorio propio
- Practicas cuando quieras, sin límites de tiempo ni de sesión de una plataforma.
- Controlas las versiones y puedes romper y reconstruir a voluntad.
- Aprendes Docker y redes de contenedores de paso (ver [[Redes de contenedores (Docker y Kubernetes)]]).
- Evitas el riesgo de atacar por error algo que no es tuyo.

### Cómo aíslas los contenedores
Hay dos ideas clave que debes combinar:

1. **Publicar puertos solo en `127.0.0.1`.** Si publicas `-p 3000:3000`, Docker escucha en *todas* las interfaces y cualquiera en tu red (o en una Wi-Fi pública) puede entrar a la app vulnerable. Publica siempre como `-p 127.0.0.1:3000:3000`.
2. **Red interna sin salida.** Para que los contenedores no salgan a internet, usa una red marcada como interna.

```mermaid
flowchart LR
  subgraph host[Tu equipo]
    subgraph red[Red interna: internal]
      JS[Juice Shop] --- ZAP[OWASP ZAP]
    end
  end
  host -. solo 127.0.0.1 .- JS
  red -. sin ruta a internet .- X[(Internet)]
```

En la línea de comandos, una red interna se crea así:

```bash
# Red sin acceso a internet para los contenedores del laboratorio
docker network create --internal lab-seguridad
```

### Cómo empezar paso a paso
1. Instala Docker Desktop (verifica sus condiciones de licencia) o Docker Engine en Linux/WSL.
2. Crea una carpeta para el laboratorio y un archivo `docker-compose.yml`.
3. Define cada app con su puerto en `127.0.0.1` y, si no necesita internet, en una red con `internal: true`.
4. Levanta con `docker compose up -d` y entra por `http://localhost:<puerto>`.
5. Al terminar, `docker compose down` para limpiar.

Tienes la versión guiada en el [[Lab 1 - Laboratorio propio con Docker Compose y red aislada]].

### Ejemplo
Un `docker-compose.yml` mínimo con Juice Shop accesible solo desde tu equipo:

```yaml
services:
  juice-shop:
    image: bkimminich/juice-shop
    ports:
      - "127.0.0.1:3000:3000"   # solo accesible desde tu máquina
    networks: [lab]
networks:
  lab:
    internal: true   # sin ruta a internet
```

### Cuándo usarlo y cuándo no
Úsalo cuando quieras un entorno permanente y bajo tu control, o para practicar con herramientas como OWASP ZAP contra una app local. Si solo quieres resolver retos puntuales sin instalar nada, usa [[OWASP Juice Shop]] en la nube de otros, [[Play with Docker]] o la PortSwigger Academy. Tu laboratorio brilla cuando repites, automatizas y combinas varias apps.

### Errores comunes
- **Publicar como `-p 3000:3000`.** Escucha en todas las interfaces; alguien en tu red podría entrar a la app vulnerable. Usa `127.0.0.1:`.
- **Olvidar el `internal: true`.** Una app vulnerable con salida a internet es más riesgo del necesario.
- **Dejar el laboratorio encendido.** Apágalo con `docker compose down` cuando no lo uses.
- **Montarlo en un servidor público.** Estas apps nunca deben vivir fuera de tu máquina.

### Para profundizar
- [Docker: redes (networking overview)](https://docs.docker.com/network/)
- [Docker Compose (referencia)](https://docs.docker.com/compose/)
- [OWASP Juice Shop (proyecto)](https://owasp.org/www-project-juice-shop/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Por qué montar un laboratorio propio?
> Para practicar con apps vulnerables (Juice Shop, DVWA, WebGoat) y herramientas (ZAP) sin depender de una plataforma y sin exponer nada a internet.

> [!question]- ¿Cómo aíslas los contenedores?
> Publicando puertos solo en `127.0.0.1` (`-p 127.0.0.1:3000:3000`) y, para que no salgan a internet, usando una red `docker network create --internal`. Con Docker Compose: `internal: true` en la red.

> [!question]- Error común.
> Publicar el puerto como `-p 3000:3000`, que escucha en todas las interfaces: cualquier equipo de tu red (o de una Wi-Fi pública) podría entrar a la app vulnerable.

## Práctica

- [[Lab 1 - Laboratorio propio con Docker Compose y red aislada]]

