---
proyecto: Conocimiento
tipo: concepto
nivel: avanzado
progreso: pendiente
estado: pendiente
area: Automatización, DevOps y operación
tags: [automatizacion-devops]
---
# Pruebas de carga (k6 y NBomber)

Parte de: [[08-Automatizacion-DevOps/02 Operación en producción/_Índice Operación en producción|Operación en producción]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Las **pruebas de carga** (y sus variantes de rendimiento) simulan muchos usuarios o requests concurrentes contra tu sistema para medir **cómo se comporta bajo tráfico**: latencia, tasa de errores, *throughput* y uso de recursos. Responden preguntas como «¿aguanta el cierre de mes?», «¿cuántos usuarios soporta este plan de App Service?» o «¿dónde está el cuello de botella?». Dos herramientas populares: **k6** (de Grafana Labs, scripts en JavaScript, de código abierto; su versión 2.0 salió en mayo de 2026) y **NBomber** (escenarios en C#/.NET).

### Cómo funciona
**Tipos de prueba**:
- **Carga** (*load*): tráfico **esperado** normal y pico, durante un rato. ¿Cumplimos el SLO con la carga real?
- **Estrés** (*stress*): subir la carga **más allá** de lo esperado hasta que el sistema se degrade o falle. ¿Dónde está el límite y cómo falla (con elegancia o colapsando)?
- **Soak** (resistencia, *endurance*): carga normal sostenida **durante horas**. Detecta fugas de memoria, conexiones que no se liberan, logs que llenan el disco.
- **Spike** (pico): un salto **súbito** de tráfico (de 10 a 500 usuarios en segundos). ¿El autoescalado reacciona a tiempo?, ¿se recupera después?
- **Smoke**: carga mínima para verificar que el script y el ambiente funcionan antes de lo demás.

**Conceptos de k6**:
- **VUs** (usuarios virtuales) que ejecutan la función por defecto en ciclo, y **escenarios** o **stages** para dar forma a la carga (rampas).
- **Checks**: validaciones por respuesta («status 200»), que no detienen la prueba.
- **Thresholds**: **criterios de aprobado/reprobado** sobre métricas agregadas, por ejemplo `http_req_duration: ['p(95)<400']` o `http_req_failed: ['rate<0.01']`. Si un threshold no se cumple, k6 termina con **código de salida distinto de 0**, así que puedes usarlo en un pipeline para **bloquear** un despliegue que empeora el rendimiento.

### Ejemplo
Prueba de carga con k6 para [[Itzamna.Dionisio]]:

```javascript
import http from 'k6/http';
import { check, sleep } from 'k6';

export const options = {
  stages: [
    { duration: '2m', target: 50 },   // rampa de subida
    { duration: '10m', target: 50 },  // carga sostenida
    { duration: '1m', target: 0 },    // bajada
  ],
  thresholds: {
    http_req_failed: ['rate<0.01'],     // menos de 1 % de errores
    http_req_duration: ['p(95)<400'],   // p95 por debajo de 400 ms
  },
};

const BASE = __ENV.BASE_URL;            // nunca apuntes a producción por accidente

export default function () {
  const res = http.get(`${BASE}/api/pedidos?pagina=1`, {
    headers: { Authorization: `Bearer ${__ENV.TOKEN}` },
  });
  check(res, { 'status 200': (r) => r.status === 200 });
  sleep(1);                             // tiempo de "pensar" del usuario
}
```

```bash
k6 run -e BASE_URL=https://app-dionisio-qa.azurewebsites.net -e TOKEN="$TOKEN" carga.js
```

Lo mismo en NBomber, para quien prefiere C#:

```csharp
using NBomber.CSharp;
using NBomber.Http.CSharp;

using var http = new HttpClient();

var escenario = Scenario.Create("consultar_pedidos", async context =>
{
    var request = Http.CreateRequest("GET", "https://app-dionisio-qa.azurewebsites.net/api/pedidos?pagina=1")
                      .WithHeader("Accept", "application/json");
    return await Http.Send(http, request);
})
.WithLoadSimulations(
    Simulation.RampingInject(rate: 50, interval: TimeSpan.FromSeconds(1), during: TimeSpan.FromMinutes(2)),
    Simulation.Inject(rate: 50, interval: TimeSpan.FromSeconds(1), during: TimeSpan.FromMinutes(10)));

NBomberRunner.RegisterScenarios(escenario).Run();
```

**¿k6 o NBomber?** k6 es gratuito y de código abierto, muy usado, con salida a Grafana y OpenTelemetry e integración sencilla en CI; los scripts son JavaScript, que el usuario ya conoce. NBomber permite escribir escenarios en C# reutilizando tus clientes y modelos, y probar protocolos no HTTP; pero según su sitio es **gratis solo para uso personal y académico**: para uso en una organización requiere licencia de pago. Para un equipo .NET en una empresa, k6 suele ser la opción por defecto.

**Azure SQL serverless y el primer minuto**: si la base está en el nivel *serverless* con **auto-pausa**, tras un periodo sin actividad se pausa; la primera conexión la **reanuda** y eso puede tardar del orden de un minuto, con errores de conexión mientras tanto. Además, el cómputo escala gradualmente según la carga y la caché de datos empieza fría. Resultado: los primeros minutos de la prueba muestran errores y latencias que no representan el estado estable. Solución: un **calentamiento** previo (fase *smoke* o rampa larga que no se evalúa), desactivar la auto-pausa durante la prueba, reintentos en la conexión ([[Azure SQL serverless (auto-pausa y reintentos)]]) y excluir ese periodo del análisis o evaluarlo aparte.

### Cuándo usarlo y cuándo no
- **Úsalo** antes de eventos de tráfico conocido, al cambiar de plan o arquitectura, y como prueba de regresión de rendimiento en el pipeline (una prueba corta con thresholds).
- **No pruebes contra producción** sin coordinación, ni contra servicios de terceros sin permiso (puedes violar sus términos o generar costos).

### Errores comunes
1. **Ambiente distinto de producción** (otro plan, base vacía) y extrapolar resultados.
2. **Generar la carga desde una sola máquina** que se satura antes que el sistema (el cuello de botella es el generador). Vigila CPU y red del generador o usa ejecución distribuida (k6 Operator, Grafana Cloud k6, Azure Load Testing).
3. **Mirar promedios**: usa percentiles (p95, p99).
4. **No tener criterios de éxito** (thresholds) definidos antes.
5. **Datos irreales**: todos los VUs consultan el mismo registro y la caché hace que todo parezca rapidísimo.

### Para profundizar
- [Documentación de Grafana k6](https://grafana.com/docs/k6/latest/)
- [Thresholds en k6](https://grafana.com/docs/k6/latest/using-k6/thresholds/)
- [Documentación de NBomber](https://nbomber.com/docs/getting-started/overview/)
- [Azure SQL Database serverless](https://learn.microsoft.com/azure/azure-sql/database/serverless-tier-overview)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- Diferencia entre prueba de carga, estrés, *soak* y *spike*.
> **Carga**: tráfico esperado, ¿cumple objetivos? **Estrés**: subir hasta romper, ¿dónde y cómo falla? **Soak**: carga sostenida horas, ¿hay fugas de memoria o conexiones? **Spike**: picos súbitos, ¿escala a tiempo?

> [!question]- ¿Qué son los *thresholds* en k6?
> Criterios de éxito en `options`: p. ej. `http_req_duration: ['p(95)<500']` y `http_req_failed: ['rate<0.01']`. Si no se cumplen, k6 termina con código distinto de 0 y el pipeline falla.

> [!question]- ¿k6 o NBomber?
> **k6**: scripts en JavaScript, binario único, muy usado en CI. **NBomber**: escenarios en C#/F#, natural para equipos .NET que quieren reutilizar código y clientes.

> [!question]- Error común al hacer pruebas de carga.
> Probar contra producción sin avisar, desde la misma máquina que el servidor, o mirar solo el promedio. Usa un ambiente parecido, genera carga desde fuera y mira **percentiles** (p95, p99) y tasa de error.

> [!question]- ¿Por qué tu prueba contra Azure SQL serverless da mal el primer minuto?
> Si la base estaba en auto-pausa, las primeras conexiones fallan mientras reanuda. Calienta antes de medir o excluye el periodo de calentamiento.

## Práctica

- [[Lab 4 - Prueba de carga con k6]]

