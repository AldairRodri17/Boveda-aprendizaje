---
proyecto:
  - Pochteca.Nomisma
proyectos:
  - "[[Pochteca.Nomisma]]"
tipo: herramienta
nivel: intermedio
estado: completo
tags:
  - conocimiento
  - backend
  - pagos
  - openpay
  - mexico
progreso: pendiente
area: Integraciones
---
# OpenPay

> Pasarela de pagos mexicana (de BBVA) con API REST: tarjeta, pago en tienda (OXXO y otras) y SPEI. [[Pochteca.Nomisma]] la llama directo con `HttpClient`.

## Qué es y qué problema resuelve
Igual que [[Stripe]]: tokeniza la tarjeta en el navegador (OpenPay.js) y el back crea el cargo con la llave privada. Es común en México por su liga con bancos locales.

## Cómo funciona por dentro
- **Base**: `https://sandbox-api.openpay.mx/v1/{merchantId}/` (o `api.openpay.mx` en producción).
- **Auth**: Basic con la llave privada como usuario y contraseña vacía (`sk_…:`).
- **Cargo**: `POST /charges` con `method`:
  - `card` + `source_id` (token) + `device_session_id` (antifraude).
  - `store`: regresa `payment_method.reference` y `barcode_url`, con `due_date`.
  - `bank_account`: regresa `clabe` y `bank` para SPEI.
- **Estatus**: `in_progress`, `charge_pending`, `completed`, `failed`, `cancelled`, `refunded`.
- **Webhooks**: se dan de alta en el panel; primero llega un evento `verification` con un código que hay que capturar en el panel. La entrega se autentica con **Basic auth** (OpenPay no firma el cuerpo).
- **Errores**: JSON con `description`, `error_code` y HTTP 4xx/5xx.

## Cuándo usarla y cuándo no
### Ventajas
- Métodos locales (tiendas, SPEI) y soporte en México.
- API sencilla.
### Desventajas
- SDK oficial de .NET Framework, no de .NET moderno.
- Webhooks sin firma del cuerpo; no permite cancelar cargos (un voucher solo vence).

## Alternativas
| Alternativa | En qué se diferencia |
|---|---|
| [[Stripe]] | SDK moderno, webhooks firmados, más métodos internacionales |
| Conekta / Mercado Pago | Otras pasarelas con OXXO y SPEI |

## Cómo se usó en Pochteca.Nomisma
- `OpenPayStrategy` (implementa `IPasarelaPagoOpenPay`) con `HttpClient` tipado (`AddHttpClient<OpenPayStrategy>()`), timeout 90 s.
- La IP del cliente va en `X-Forwarded-For` para el antifraude.
- 401/403 → 502, 429/5xx/timeout → 503, 404 → 404; un rechazo al crear es un resultado fallido ([[Cargos OpenPay - Back]]).
- Webhook con Basic auth comparada en tiempo constante y captura del código de verificación ([[Webhook OpenPay - Back]]).

```csharp
// Pochteca.Nomisma.Infrastructure/Payments/OpenPay/Strategies/OpenPayStrategy.cs
_chargesUri = new Uri(new Uri(apiBase), $"v1/{Uri.EscapeDataString(merchantId)}/charges");
_authorization = new AuthenticationHeaderValue(
    "Basic",
    Convert.ToBase64String(Encoding.UTF8.GetBytes($"{privateKey}:")));
```

Funcionalidades: [[Cargos OpenPay]], [[Webhook OpenPay]].

## Errores comunes y buenas prácticas
- Olvidar `device_session_id` en tarjeta: OpenPay rechaza por antifraude.
- Dar de alta el webhook sin capturar el código de verificación.
- Tratar `charge_pending` (OXXO/SPEI recién creado) como pago hecho.

## Qué estudiar después
- [[Webhooks]], [[Patrón Strategy]].

## Documentación oficial
- https://documents.openpay.mx/docs/api/
- https://documents.openpay.mx/docs/webhooks.html

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo se autentica la API de OpenPay?
> Con Basic auth usando la llave privada como usuario y contraseña vacía (`sk_…:`), sobre la URL que incluye el `merchantId`.

> [!question]- ¿Qué campo es obligatorio para cobros con tarjeta y para qué sirve?
> `device_session_id`, generado por la librería JS de antifraude; sin él el cargo puede rechazarse.

> [!question]- ¿Cómo se valida un webhook de OpenPay?
> OpenPay no firma el cuerpo: la entrega se autentica con Basic auth configurada en el panel (compárala en tiempo constante) y al darlo de alta hay que capturar el código del evento `verification`.

> [!question]- Error común: tratar `charge_pending` como pagado.
> Es el estado de un OXXO/SPEI recién creado; el pago real llega después con el evento correspondiente.

