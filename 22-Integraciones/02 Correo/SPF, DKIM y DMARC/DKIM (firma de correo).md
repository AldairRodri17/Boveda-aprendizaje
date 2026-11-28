---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Integraciones
tags: [integraciones]
contenido: completo
---
# DKIM (firma de correo)

Parte de: [[SPF, DKIM y DMARC]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**DKIM** (*DomainKeys Identified Mail*, RFC 6376) permite que un dominio **firme criptográficamente** los correos que envía. El servidor emisor calcula una firma sobre ciertas cabeceras y el cuerpo con una **llave privada**, y publica la **llave pública** en DNS. El receptor la consulta y verifica dos cosas: que el mensaje **no se alteró** en el camino y que lo firmó alguien autorizado por el dominio indicado en `d=`. A diferencia de SPF, la firma viaja con el mensaje, así que **sobrevive a reenvíos** mientras no se modifique el contenido firmado.

### Cómo funciona
```mermaid
sequenceDiagram
  participant E as Servidor emisor (proveedor)
  participant DNS
  participant R as Receptor (Gmail)
  E->>E: hash de cabeceras y cuerpo, firma con llave privada
  E->>R: correo con cabecera DKIM-Signature (d=midominio.com; s=s1)
  R->>DNS: TXT s1._domainkey.midominio.com
  DNS-->>R: v=DKIM1; k=rsa; p=MIIBIjANBgkq...
  R->>R: verifica firma y hash del cuerpo
  R-->>R: dkim=pass (d=midominio.com)
```
Partes de la cabecera `DKIM-Signature`:

- `v=1`: versión. `a=rsa-sha256`: algoritmo (también existe `ed25519-sha256`).
- `d=`: el **dominio que firma**; es el que DMARC compara para la alineación.
- `s=`: el **selector**.
- `h=`: las cabeceras firmadas (From, Subject, Date...). `bh=`: hash del cuerpo. `b=`: la firma.
- `c=relaxed/relaxed`: canonicalización; *relaxed* tolera cambios menores de espacios y mayúsculas en cabeceras.

**El selector** es un nombre arbitrario que permite tener **varias llaves** para el mismo dominio: una por proveedor (Microsoft 365 usa `selector1` y `selector2`, tu proveedor transaccional otro) y facilita la **rotación**: publicas la llave nueva con otro selector, cambias la firma y luego retiras la vieja. La llave pública vive siempre en `<selector>._domainkey.<dominio>`.

### Ejemplo
Para la cabecera `DKIM-Signature: v=1; a=rsa-sha256; d=midominio.com; s=s1; …` el registro a consultar es el TXT de **`s1._domainkey.midominio.com`**:

En Windows:
```powershell
Resolve-DnsName -Type TXT s1._domainkey.midominio.com
nslookup -type=TXT s1._domainkey.midominio.com
```
En Linux o macOS:
```bash
dig +short TXT s1._domainkey.midominio.com
```
Respuesta típica: `"v=DKIM1; k=rsa; p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA..."`. Si `p=` está vacío, la llave fue revocada.

En la práctica no generas llaves tú: el proveedor (Azure Communication Services, Microsoft 365, Resend, SES) te da dos o tres registros **CNAME** o TXT que publicas en tu DNS (por ejemplo, en Azure DNS o Cloudflare); los CNAME permiten que el proveedor rote las llaves sin que toques tu zona. Para verificar un correo real, en Gmail abre "Mostrar original" y busca `dkim=pass header.d=midominio.com`.

**Qué pasa si un reenvío modifica el cuerpo**: una lista de correo que agrega un pie ("Para darte de baja...") o cambia el asunto con `[lista]` rompe el hash y la firma da `dkim=fail`. Si además SPF falla (porque el reenviador envía desde su IP), DMARC falla. Mitigaciones: las listas modernas reescriben el `From` o firman con su propio dominio, y existe **ARC** (*Authenticated Received Chain*, RFC 8617), que permite a los intermediarios dejar constancia de que el mensaje autenticaba bien antes de modificarlo.

### Cuándo usarlo y cuándo no
- **Siempre** firma con DKIM todo correo de tu dominio, transaccional y corporativo; es requisito de Gmail, Yahoo y Outlook para remitentes masivos.
- Usa llaves RSA de **2048 bits**; 1024 se considera débil.
- Firma con **tu dominio** (`d=midominio.com` o un subdominio), no con el dominio del proveedor, para que DMARC alinee.
- Combínalo con [[SPF (Sender Policy Framework)]] y publica [[DMARC (política y reportes)]]; el panorama está en [[SPF, DKIM y DMARC]].

### Errores comunes
1. **Dejar la firma con el dominio del proveedor** (`d=proveedor.com`): DKIM pasa, pero no alinea con tu `From` y DMARC falla.
2. **Registros TXT partidos mal** en el DNS: las llaves de 2048 bits superan 255 caracteres y deben dividirse en varias cadenas dentro del mismo registro.
3. **Borrar el selector viejo antes de tiempo** durante una rotación: correos en tránsito fallan.
4. **Modificar el mensaje después de firmar** (un gateway que agrega un aviso legal).

### Para profundizar
- [RFC 6376: DKIM](https://www.rfc-editor.org/rfc/rfc6376)
- [RFC 8617: ARC](https://www.rfc-editor.org/rfc/rfc8617)
- [Configurar DKIM en Microsoft 365](https://learn.microsoft.com/defender-office-365/email-authentication-dkim-configure)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- La cabecera dice `DKIM-Signature: v=1; a=rsa-sha256; d=midominio.com; s=s1; …`. ¿Qué registro DNS consultas?
> TXT de `s1._domainkey.midominio.com`.

> [!question]- ¿Cómo lo consultas?
> `nslookup -type=txt s1._domainkey.midominio.com` o `dig TXT s1._domainkey.midominio.com +short`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo funciona DKIM?
> El servidor de salida firma encabezados y cuerpo con una llave privada; el receptor busca la llave pública en DNS en `selector._domainkey.dominio` y verifica la firma.

> [!question]- ¿Qué es el selector?
> Un nombre que permite tener varias llaves (por proveedor o para rotarlas); aparece en la firma como `s=`.

> [!question]- ¿Qué pasa si un reenvío modifica el cuerpo del mensaje?
> La firma DKIM falla. Por eso se recomienda que DMARC pueda pasar por SPF **o** por DKIM alineado.

## Práctica

- [[Lab 2 - Verificar SPF, DKIM y DMARC de tu dominio]]

