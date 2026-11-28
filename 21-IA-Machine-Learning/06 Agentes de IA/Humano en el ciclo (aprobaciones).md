---
proyecto: Conocimiento
tipo: concepto
nivel: avanzado
progreso: pendiente
estado: pendiente
area: IA y Machine Learning
tags: [ia-machine-learning]
contenido: completo
---
# Humano en el ciclo (aprobaciones)

Parte de: [[21-IA-Machine-Learning/06 Agentes de IA/_Índice Agentes de IA|Agentes de IA]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Humano en el ciclo** (*human in the loop*, HITL) significa que el agente **se detiene y pide permiso** a una persona antes de ejecutar ciertas acciones, o le pide información que le falta. El agente propone; el humano aprueba, rechaza o corrige; y el agente continúa con esa decisión. Es una de las defensas más efectivas contra errores del modelo y contra [[Prompt injection]], porque una instrucción maliciosa no llega a tener efecto sin que alguien la vea.

### Cómo funciona
1. Marcas ciertas herramientas como "requieren aprobación".
2. Cuando el modelo pide llamar una de ellas, el framework **no la ejecuta**: devuelve una *solicitud de aprobación* con el nombre de la herramienta y los argumentos.
3. Tu aplicación muestra la solicitud al usuario de forma clara (qué se va a hacer, sobre qué datos, con qué impacto).
4. El usuario aprueba o rechaza; tu aplicación manda esa respuesta al agente **en la misma sesión**.
5. Si se aprobó, se ejecuta la herramienta; si se rechazó, el modelo recibe el rechazo y debe buscar otra vía o explicar.

```mermaid
sequenceDiagram
    participant U as Usuario
    participant App
    participant Ag as Agente
    App->>Ag: "Cancela la factura F-889"
    Ag-->>App: Solicitud de aprobación: CancelarFactura(F-889)
    App->>U: ¿Aprobar cancelación de F-889 por $12,400?
    U->>App: Aprobar
    App->>Ag: Respuesta: aprobado (misma sesión)
    Ag-->>App: "Factura cancelada; folio de acuse..."
```

#### Qué acciones deberían requerir aprobación
Decide por **impacto y reversibilidad**, no por "todo lo que suene importante":
- **Irreversibles o difíciles de deshacer**: borrar datos, cancelar facturas, enviar correos o mensajes a clientes, publicar, hacer *merge* o desplegar.
- **Con dinero de por medio**: pagos, reembolsos, compras, cambios de precio.
- **Que salen de tu sistema**: llamadas a APIs externas con datos sensibles, compartir archivos.
- **Que cambian permisos o configuración**: altas de usuarios, roles, secretos.
- **Por encima de un umbral**: por ejemplo reembolsos mayores a cierto monto.

Las acciones de **solo lectura** y de bajo riesgo (consultar un pedido, buscar en documentación) normalmente no requieren aprobación.

### Ejemplo
En Microsoft Agent Framework (C#) envuelves la función con `ApprovalRequiredAIFunction`; el agente devuelve contenido `ToolApprovalRequestContent` que tú respondes:

```csharp
using System.ComponentModel;
using Microsoft.Agents.AI;
using Microsoft.Extensions.AI;

[Description("Cancela una factura emitida.")]
static string CancelarFactura([Description("Folio")] string folio) => $"Factura {folio} cancelada";

AIFunction cancelar = new ApprovalRequiredAIFunction(AIFunctionFactory.Create(CancelarFactura));
AIAgent agente = chatClient.AsAIAgent(
    instructions: "Eres asistente de facturación.",
    tools: [cancelar]);

AgentSession sesion = await agente.CreateSessionAsync();
AgentResponse r = await agente.RunAsync("Cancela la factura F-889", sesion);

var solicitudes = r.Messages.SelectMany(m => m.Contents).OfType<ToolApprovalRequestContent>().ToList();
foreach (var s in solicitudes)
{
    var llamada = (FunctionCallContent)s.ToolCall;
    Console.Write($"¿Aprobar {llamada.Name}({string.Join(", ", llamada.Arguments!)})? [s/n] ");
    bool aprobado = Console.ReadLine()?.Trim().ToLower() == "s";
    var respuesta = new ChatMessage(ChatRole.User, [s.CreateResponse(aprobado)]);
    Console.WriteLine(await agente.RunAsync(respuesta, sesion));   // misma sesión
}
```

En Python, el mismo framework usa `@tool(approval_mode="always_require")`. En una API web (por ejemplo Itzamna.Dionisio con React en Itzamna.Clio), la solicitud se guarda con la sesión serializada y el front muestra un botón "Aprobar/Rechazar" que llama a un endpoint para reanudar.

### Cuándo usarlo y cuándo no
- **Úsalo** en acciones con efectos externos o irreversibles, y mientras el agente es nuevo y no tienes métricas de confianza.
- **Ajusta con el tiempo**: lo que se aprueba siempre sin cambios puede pasar a automático con monitoreo; lo que se rechaza seguido indica un problema de diseño.
- **No lo uses** como sustituto de permisos: si una acción nunca debe ocurrir, no le des la herramienta (ver [[Guardrails y límites de un agente]]).

### Errores comunes
- **Pedir aprobación para todo**: el usuario se cansa y empieza a aprobar sin leer (*fatiga de aprobación*), con lo que la protección desaparece. Reserva las aprobaciones para lo que de verdad importa.
- **Solicitudes poco claras**: mostrar `{"id":889}` en vez de "Cancelar factura F-889 de Cliente X por $12,400". El humano debe entender el impacto.
- **Perder la sesión entre la solicitud y la respuesta**: la aprobación debe aplicarse a la llamada pendiente exacta; reutiliza la misma sesión.
- **Aprobar en el prompt**: escribir "pide permiso antes de borrar" en las instrucciones no es un control; el modelo puede ignorarlo. La aprobación debe estar en el código.
- **No registrar quién aprobó**: guarda usuario, fecha y argumentos aprobados para auditoría.

### Para profundizar
- [Aprobación de herramientas en Microsoft Agent Framework (Microsoft Learn)](https://learn.microsoft.com/agent-framework/agents/tools/tool-approval)
- [OWASP Top 10 para aplicaciones LLM](https://genai.owasp.org/llm-top-10/)
- [Building effective agents (Anthropic)](https://www.anthropic.com/engineering/building-effective-agents)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es *human in the loop*?
> Pausar al agente para que una persona apruebe, corrija o rechace antes de acciones importantes.

> [!question]- ¿Qué acciones deberían requerir aprobación?
> Las irreversibles o de impacto: enviar correos, pagar, borrar datos, desplegar, cambiar permisos.

> [!question]- Error común: pedir aprobación para todo.
> La gente aprueba sin leer (fatiga). Reserva la aprobación para lo riesgoso y muestra un resumen claro de lo que se hará.

## Práctica

- [[Lab 6 - Agente con tool calling y un servidor MCP sencillo]]

