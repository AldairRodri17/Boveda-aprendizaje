---
proyecto: Conocimiento
tipo: laboratorio
nivel: avanzado
progreso: pendiente
estado: borrador
area: Automatización, DevOps y operación
herramienta: k6 (gratuito, local o en Docker) y la API del Lab 1
tags: [automatizacion-devops, laboratorio]
contenido: completo
---
# Lab 4 - Prueba de carga con k6

Parte de: [[08-Automatizacion-DevOps/02 Operación en producción/_Índice Operación en producción|Operación en producción]]

**Cubre:** [[Pruebas de carga (k6 y NBomber)]] · [[SLO (objetivo de nivel de servicio)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Escribir una prueba de carga con etapas y *thresholds* alineados a un SLO, interpretar percentiles y encontrar el punto donde la API deja de cumplir.

## Pasos

1. Levanta la API con SQL del Lab 1 (`docker compose up -d`).
2. Instala k6 (`winget install k6 --source winget` o usa la imagen `grafana/k6`).
3. Escribe `carga.js`: etapas de 10 → 50 usuarios virtuales en ~4 min contra `/db`, con `check` de status 200 y `sleep(1)`.
4. Define thresholds: menos de 1 % de errores y p95 < 300 ms. Lee la URL base de una variable de entorno.
5. Ejecuta y anota p50, p95, p99, requests/s y tasa de error.
6. Cambia el threshold a `p(95)<1` y comprueba que k6 termina con código distinto de 0 (como fallaría en un pipeline).
7. Haz una prueba de estrés subiendo hasta 200 o más usuarios y anota en qué carga se rompe el threshold de p95 y qué recurso se satura (`docker stats`).
8. Escribe tus conclusiones en Notas personales: ¿con qué carga cumplirías un SLO de p95 < 300 ms?

## Criterios de terminado

- [ ] La prueba base termina con todos los thresholds en verde (o explicas por qué no).
- [ ] Tienes registrados p95, p99 y tasa de error de cada corrida.
- [ ] Con el threshold imposible, el código de salida es distinto de 0 (`echo $?` o `$LASTEXITCODE`).
- [ ] Identificaste la carga aproximada en la que se incumple p95 < 300 ms.
- [ ] Anotaste qué se saturó primero (CPU de la API, de SQL u otra cosa).

## Solución

> [!success]- Solución: carga.js
> ```javascript
> import http from 'k6/http';
> import { check, sleep } from 'k6';
>
> export const options = {
>   stages: [
>     { duration: '30s', target: 10 },
>     { duration: '1m', target: 10 },
>     { duration: '30s', target: 50 },
>     { duration: '1m', target: 50 },
>     { duration: '30s', target: 0 },
>   ],
>   thresholds: {
>     http_req_failed: ['rate<0.01'],
>     http_req_duration: ['p(95)<300'],
>   },
> };
>
> const BASE = __ENV.BASE_URL || 'http://localhost:5000';
>
> export default function () {
>   const r = http.get(`${BASE}/db`);
>   check(r, { 'status 200': (x) => x.status === 200 });
>   sleep(1);
> }
> ```

> [!success]- Solución: ejecución
> ```bash
> k6 run -e BASE_URL=http://localhost:5000 carga.js
> echo $?          # PowerShell: $LASTEXITCODE
> # Con Docker (la API está en el host):
> docker run --rm -i -e BASE_URL=http://host.docker.internal:5000 grafana/k6 run - < carga.js
> ```
> Si un threshold falla, k6 sale con código distinto de 0 y el pipeline marca el paso como fallido. Para estrés, cambia las etapas a 50 → 100 → 200 → 400 usuarios y fíjate en `http_req_duration` p(95) y en `docker stats`. Mira percentiles, no el promedio: un p99 alto con un promedio bueno indica que algunos usuarios sufren mucho.

## Notas personales

_Qué me costó, errores que cometí, dudas._
