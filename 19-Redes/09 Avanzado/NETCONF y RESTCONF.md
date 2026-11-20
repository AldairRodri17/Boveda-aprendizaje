---
proyecto: Conocimiento
tipo: concepto
nivel: avanzado
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# NETCONF y RESTCONF

Parte de: [[19-Redes/09 Avanzado/_Índice Avanzado|Avanzado]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**NETCONF** y **RESTCONF** son protocolos estándar del IETF para **configurar y consultar equipos de red mediante APIs**, con datos **estructurados** en lugar de texto de la CLI. Ambos se basan en modelos de datos escritos en **YANG**, que describen formalmente qué se puede configurar en un equipo (interfaces, rutas, VLAN) y con qué tipos. Son la base de la programabilidad moderna en Cisco IOS XE, NX-OS, IOS XR y otros fabricantes.

### Cómo funciona
**YANG** (RFC 7950) es un lenguaje para modelar datos de configuración y estado. Un modelo define contenedores, listas y hojas con tipos y restricciones; por ejemplo, el modelo estándar `ietf-interfaces` define que una interfaz tiene `name`, `description`, `enabled`, etc. Hay modelos **estándar** (IETF, OpenConfig) y **nativos** del fabricante (Cisco-IOS-XE-native). YANG no es un protocolo: es el "esquema", como un contrato OpenAPI o un esquema de base de datos.

**NETCONF** (RFC 6241):
- Transporte: **SSH**, puerto **830**.
- Codificación: **XML**.
- Modelo **RPC**: operaciones como `<get>`, `<get-config>`, `<edit-config>`, `<lock>`, `<commit>`, `<copy-config>`, `<delete-config>`.
- **Datastores**: `running`, y en equipos que lo soportan `candidate` (preparas los cambios y los aplicas todos juntos con `commit`, de forma transaccional) y `startup`.
- Ventaja: transacciones y validación; ideal para orquestadores.

**RESTCONF** (RFC 8040):
- Transporte: **HTTPS**.
- Codificación: **JSON** o XML.
- Usa los **verbos HTTP** sobre recursos derivados de los modelos YANG: `GET` (leer), `POST` (crear), `PUT` (crear o reemplazar), `PATCH` (modificar), `DELETE`.
- URL típica: `https://<equipo>/restconf/data/<modelo>:<contenedor>`.
- Ventaja: se usa con cualquier cliente HTTP (curl, Postman, `HttpClient` de .NET); muy familiar para un desarrollador web.

| | NETCONF | RESTCONF |
|---|---|---|
| Transporte | SSH (830) | HTTPS (443) |
| Formato | XML | JSON o XML |
| Operaciones | RPC (`edit-config`, `get`) | Verbos HTTP |
| Transacciones con candidate | Sí | Limitado |
| Modelos | YANG | YANG |

### Ejemplo
Habilitar las APIs en un router IOS XE:

```text
R1(config)# netconf-yang
R1(config)# restconf
R1(config)# ip http secure-server
R1(config)# username api privilege 15 secret <ClaveApi>
```

Consultar las interfaces con RESTCONF desde la terminal (las credenciales en variables, `-k` solo en laboratorio con certificado autofirmado):

```bash
curl -k -u "$NET_USER:$NET_PASS" \
  -H "Accept: application/yang-data+json" \
  https://10.99.0.1/restconf/data/ietf-interfaces:interfaces
```

Cambiar la descripción de una interfaz con PATCH desde C#:

```csharp
using System.Net.Http.Headers;
using System.Text;

var handler = new HttpClientHandler(); // en producción, valida el certificado del equipo
using var http = new HttpClient(handler) { BaseAddress = new Uri("https://10.99.0.1/") };
var token = Convert.ToBase64String(Encoding.UTF8.GetBytes(
    $"{Environment.GetEnvironmentVariable("NET_USER")}:{Environment.GetEnvironmentVariable("NET_PASS")}"));
http.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", token);

var json = """
{ "ietf-interfaces:interface": { "name": "GigabitEthernet2", "description": "Enlace a SW1" } }
""";
var contenido = new StringContent(json, Encoding.UTF8, "application/yang-data+json");
var respuesta = await http.PatchAsync(
    "restconf/data/ietf-interfaces:interfaces/interface=GigabitEthernet2", contenido);
Console.WriteLine((int)respuesta.StatusCode); // 204 si se aplicó
```

Con NETCONF en Python se usa la biblioteca `ncclient`, enviando `edit-config` con un XML basado en el mismo modelo.

Para practicar sin equipo propio, Cisco DevNet ofrece sandboxes con IOS XE y las APIs habilitadas.

### Cuándo usarlo y cuándo no
Úsalos cuando automatices equipos que los soportan (IOS XE moderno y la mayoría de plataformas actuales): obtienes datos estructurados, validación y operaciones más seguras que enviar comandos de texto por SSH. Para equipos antiguos sin APIs, sigues con SSH y Netmiko. Para métricas en tiempo real se complementan con telemetría por streaming (gNMI).

### Errores comunes
- **Confundir YANG con un protocolo**: es el modelo de datos.
- **Olvidar el encabezado** `application/yang-data+json` en RESTCONF.
- **Usar PUT cuando querías PATCH**: PUT reemplaza el recurso completo y puede borrar configuración.
- **Deshabilitar la validación de certificados** en producción.

### Para profundizar
- [RFC 6241: NETCONF](https://www.rfc-editor.org/rfc/rfc6241)
- [RFC 8040: RESTCONF](https://www.rfc-editor.org/rfc/rfc8040)
- [RFC 7950: YANG 1.1](https://www.rfc-editor.org/rfc/rfc7950)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es NETCONF?
> Un protocolo para configurar equipos con datos estructurados en **XML** sobre SSH (puerto **830**). Operaciones como `get`, `get-config` y `edit-config`, con datastores running/candidate/startup.

> [!question]- ¿Qué es RESTCONF?
> Una interfaz **HTTP(S)** tipo REST sobre los mismos modelos de datos, con JSON o XML y métodos GET, POST, PUT, PATCH y DELETE.

> [!question]- ¿Qué papel juega YANG?
> Es el **lenguaje de modelado** que describe la estructura de configuración y estado que exponen NETCONF y RESTCONF.

