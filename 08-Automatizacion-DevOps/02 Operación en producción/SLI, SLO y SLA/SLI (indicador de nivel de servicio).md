---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Automatización, DevOps y operación
tags: [automatizacion-devops]
contenido: completo
---
# SLI (indicador de nivel de servicio)

Parte de: [[SLI, SLO y SLA]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **SLI** (*Service Level Indicator*) es una **medición cuantitativa de algún aspecto del servicio que le importa al usuario**: ¿respondió?, ¿respondió rápido?, ¿respondió correctamente? Es la base de los [[SLO (objetivo de nivel de servicio)|SLO]]: sin un buen indicador, cualquier objetivo es humo. Forma parte de [[SLI, SLO y SLA]].

### Cómo funciona
**Cómo se expresa un buen SLI**: como una **proporción de eventos buenos sobre eventos válidos**, en porcentaje:

```text
SLI = eventos buenos / eventos válidos × 100
```

- *Disponibilidad*: requests que no devolvieron 5xx / todos los requests válidos.
- *Latencia*: requests que respondieron en menos de 300 ms / todos los requests.
- *Calidad*: respuestas completas (sin datos degradados) / todas.
- *Frescura* (para procesos de datos): registros actualizados hace menos de 10 min / todos.
- *Corrección*: reportes generados con totales correctos / reportes generados.

Esta forma tiene ventajas: siempre va de 0 a 100 %, «más alto es mejor», y se traduce directo a un SLO y a un [[Error budgets|presupuesto de error]]. Debes definir con precisión qué es un evento **válido** (¿cuentan los 4xx causados por el cliente? normalmente no como fallas; ¿cuentan los health checks? no) y qué es **bueno**.

**Por qué la CPU no es un buen SLI**: la CPU mide **causas**, no **síntomas** que vive el usuario. Puedes tener 95 % de CPU y responder perfecto (bien dimensionado) o 20 % de CPU y estar devolviendo errores porque la base de datos no responde o un certificado expiró. Un SLI debe responder «¿el usuario está teniendo una buena experiencia?». La CPU, la memoria y las conexiones son útiles para **diagnosticar** y para planear capacidad, no para definir objetivos.

**¿Dónde se mide?** Cada punto tiene ventajas:
- **En el servidor** (logs de la API, [[Application Insights]] en el backend): fácil y preciso, pero no ve las fallas que ocurren **antes** de llegar a tu código (DNS, balanceador, la app ni siquiera arrancó).
- **En el balanceador o gateway** (Front Door, Application Gateway): ve también los requests que tu app no atendió; buen equilibrio.
- **En el cliente** (navegador con el SDK de JavaScript, app móvil): lo más cercano a la experiencia real, incluida la red del usuario, pero con más ruido (redes malas que no son tu culpa) y datos menos controlados.
- **Sondas sintéticas** (pruebas de disponibilidad desde varias regiones): constantes y comparables, pero no reflejan tráfico real.

La recomendación práctica: medir **lo más cerca posible del usuario** que puedas hacer de forma confiable; para una API, normalmente en el **balanceador** o en el servidor, complementado con pruebas sintéticas y telemetría del cliente para la latencia percibida.

### Ejemplo
SLI de latencia para el endpoint de reportes, desde los requests en Application Insights:

```kusto
requests
| where timestamp > ago(7d)
| where name == "GET /api/reportes/ventas"
| where toint(resultCode) < 500            // solo eventos válidos para latencia
| summarize validos = count(), buenos = countif(duration < 2000ms)
| extend sliLatencia = round(100.0 * buenos / validos, 3)
```

En React (Itzamna.Clio), el SDK de Application Insights para JavaScript puede registrar la duración de las llamadas `fetch` vistas por el navegador para comparar con la medida del servidor.

### Cuándo usarlo y cuándo no
- Define 1–3 SLI por servicio, alineados con lo que el usuario hace (iniciar sesión, consultar, pagar).
- No conviertas cada métrica del tablero en SLI; los demás indicadores siguen siendo útiles para diagnóstico en [[Observabilidad]].

### Errores comunes
1. **Promedios de latencia** en lugar de proporción bajo un umbral o percentiles: el promedio esconde la cola lenta.
2. **Contar health checks y bots** como tráfico válido.
3. **Medir solo en el servidor** y no ver caídas del balanceador o del DNS.
4. **Definiciones ambiguas** que cambian cada mes.

### Para profundizar
- [Google SRE Book: Service Level Objectives (indicadores)](https://sre.google/sre-book/service-level-objectives/)
- [Google SRE Workbook: Implementing SLOs](https://sre.google/workbook/implementing-slos/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo se expresa un buen SLI?
> Como proporción de eventos buenos sobre eventos válidos: `requests con código < 500 y latencia < 300 ms / requests totales`.

> [!question]- ¿Por qué la CPU no es un buen SLI?
> No es lo que vive el usuario: puede haber CPU alta sin impacto o errores con CPU baja. Los SLI miden **síntomas**: disponibilidad, latencia, frescura de datos, corrección.

> [!question]- ¿Dónde mides el SLI: en el servidor o en el cliente?
> Lo más cerca posible del usuario (balanceador, gateway, pruebas sintéticas). El servidor no ve las peticiones que ni siquiera le llegaron.

