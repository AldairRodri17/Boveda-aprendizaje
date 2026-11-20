---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Laboratorios y práctica
tags: [laboratorios-practica]
---
# Play with Docker

Parte de: [[16-Laboratorios-Practica/05 Laboratorios temporales en el navegador/_Índice Laboratorios temporales en el navegador|Laboratorios temporales en el navegador]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Play with Docker (PWD) es un entorno **gratuito en el navegador** que te da instancias Linux con Docker ya instalado, para practicar contenedores **sin instalar Docker Desktop** en tu equipo. Lo mantiene Docker como herramienta educativa: creas instancias, levantas contenedores y pruebas comandos en una terminal web.

### Qué se practica ahí
Comandos básicos e intermedios de Docker: construir imágenes, correr contenedores, publicar puertos, usar `docker compose`, crear redes y volúmenes, y hasta armar un pequeño Swarm con varias instancias. Es ideal para los primeros pasos antes de montar [[Cómo montar un laboratorio propio con Docker y red aislada]] en tu máquina.

### Cómo funciona
```mermaid
flowchart LR
  L[Login con Docker Hub] --> I[Instancia(s) Alpine con Docker]
  I --> C[docker run / build / compose]
  C --> X[Todo se borra al expirar la sesión]
```

Cada instancia es una VM Alpine temporal; puedes tener varias y conectarlas (útil para Swarm).

### Qué límites tiene
- Requiere **iniciar sesión con una cuenta de Docker Hub** (gratuita).
- Las **sesiones expiran** en pocas horas (la documentación menciona hasta 4 horas; en la práctica suele rondar las 2) y **todo se borra** al terminar.
- Puedes correr hasta ~5 instancias por sesión; los recursos son compartidos y limitados.
- La disponibilidad puede variar (verificar en la página oficial).

### Qué cuidado hay
**No subas secretos ni datos reales**: es un entorno **compartido y temporal**. Trata todo lo que hagas ahí como público y efímero. Si construyes una imagen que quieras conservar, **súbela a Docker Hub antes** de que expire la sesión, porque al terminar no queda nada.

### Cómo empezar paso a paso
1. Entra a labs.play-with-docker.com e inicia sesión con tu cuenta de Docker Hub.
2. Pulsa **Start** y luego **Add New Instance**.
3. Prueba un contenedor:

```bash
# Levantar un servidor web y mapear el puerto
docker run -d -p 8080:80 nginx
docker ps
```

4. Usa el botón del puerto (8080) que aparece arriba para abrir el servicio.
5. Si creas algo valioso, publícalo en Docker Hub antes de que la sesión expire.

### Ejemplo
Para practicar `docker compose`, puedes pegar un archivo con dos servicios (una API y una base) y levantarlos con `docker compose up`. Es el mismo concepto del [[Lab 1 - Dockerfile y Docker Compose de una API con SQL Server]], pero sin ensuciar tu equipo.

### Cuándo usarlo y cuándo no
Úsalo para practicar Docker rápido, probar un `compose` ajeno o enseñar sin pedir instalaciones. Para trabajo real y persistente, instala Docker localmente. Para entornos ligados a tu repo, [[GitHub Codespaces]]. Para Kubernetes, [[Play with Kubernetes]] o [[Killercoda]].

### Errores comunes
- **Esperar que algo persista.** Todo se borra; publica tus imágenes a Docker Hub.
- **Subir secretos o datos reales.** Entorno compartido y temporal.
- **Depender de él para demos largas.** La sesión expira a las pocas horas.
- **Olvidar el login de Docker Hub.** Sin cuenta no entras.

Un apunte sobre sus límites: por ser compartido y efímero, no sirve para medir rendimiento ni para nada que deba durar, pero es perfecto para una demo rápida, para seguir un tutorial o para probar un `docker compose` ajeno sin ensuciar tu equipo. Si ya tienes Docker instalado en tu máquina, casi siempre te conviene más trabajar en local.

### Para profundizar
- [Play with Docker](https://labs.play-with-docker.com/)
- [Documentación oficial de Docker](https://docs.docker.com/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es Play with Docker?
> Un entorno gratuito en el navegador con instancias Linux y Docker instalado, para practicar sin instalar Docker Desktop.

> [!question]- ¿Qué límites tiene?
> Requiere iniciar sesión con Docker Hub, las sesiones expiran en pocas horas y todo se borra al terminar. La disponibilidad puede variar (verificar en la página oficial).

> [!question]- ¿Qué cuidado hay?
> No subas secretos ni datos reales: es un entorno compartido y temporal.

