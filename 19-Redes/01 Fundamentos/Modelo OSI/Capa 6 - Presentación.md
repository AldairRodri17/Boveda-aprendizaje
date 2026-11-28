---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
contenido: completo
---
# Capa 6 - Presentación

Parte de: [[Modelo OSI]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La capa de presentación se ocupa de **cómo se representan los datos** para que ambas aplicaciones los entiendan igual: formato, codificación de caracteres, compresión y cifrado. Es la "traductora" del [[Modelo OSI]]. Igual que la [[Capa 5 - Sesión]], en TCP/IP no tiene protocolos propios: sus funciones las hacen las aplicaciones, las bibliotecas de serialización y TLS.

### Cómo funciona
Sus tres funciones clásicas:
1. **Traducción de formato y codificación**: convertir entre representaciones. Ejemplos: texto en UTF-8 frente a Latin-1, orden de bytes (big endian en la red frente a little endian en x86), o estructuras serializadas como JSON, XML, Protocol Buffers o ASN.1 (usado por SNMP y por los certificados X.509).
2. **Compresión**: reducir el tamaño antes de enviar (gzip o Brotli en HTTP, códecs de audio y video).
3. **Cifrado y descifrado**: proteger la confidencialidad. TLS cifra los datos de la aplicación antes de entregarlos a TCP; por eso HTTPS se describe como HTTP más funciones de capas 5 y 6.

Formatos asociados a esta capa: JPEG, PNG, GIF, MP3, MPEG/H.264, ASCII, UTF-8, EBCDIC, MIME (en el correo), JSON y XML.

### Ejemplo
Tu API de Itzamna.Dionisio responde un cliente con nombre "Peña Ñúñez":

```http
GET /api/clientes/42 HTTP/1.1
Accept: application/json
Accept-Encoding: br, gzip

HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Encoding: br
```

- `charset=utf-8` es una decisión de presentación: si el front en Itzamna.Clio interpretara los bytes como Latin-1, verías "PeÃ±a". Ese error clásico de acentos es un problema de capa 6, no de red.
- `Content-Encoding: br` indica compresión Brotli: el servidor comprime y el navegador descomprime.
- Todo viaja dentro de TLS, que cifra antes de bajar a la capa 4.

En Wee.Reporteria, un archivo Excel `.xlsx` es en realidad un ZIP con XML dentro: compresión y formato, otra vez presentación.

En equipos de red esto aparece poco, pero lo verás en SNMP (los mensajes se codifican con ASN.1/BER), en SSH (cifrado) y cuando un archivo de configuración se guarda con finales de línea de Windows y el equipo lo interpreta distinto.

### Cuándo usarlo y cuándo no
Piensa en capa 6 cuando la comunicación **sí ocurre** pero el contenido llega ilegible: caracteres raros, archivos corruptos al abrirlos, errores de deserialización, versiones de TLS o cifrados que no coinciden ("no shared cipher"), o compresión que el cliente no soporta. No sirve para explicar pérdidas de paquetes o rutas: eso es de capas inferiores.

### Errores comunes
- **Culpar a la red por los acentos rotos**: casi siempre es una codificación mal declarada en el encabezado o en la base de datos.
- **Pensar que comprimir siempre conviene**: comprimir datos ya comprimidos (JPEG, ZIP) gasta CPU sin ahorrar bytes.
- **Creer que cifrado y codificación son lo mismo**: Base64 es codificación, no protege nada; el cifrado requiere una llave.
- **Buscar "la capa 6" en Wireshark**: verás TLS o el protocolo de aplicación, no una capa con ese nombre.

### Para profundizar
- [RFC 3629: UTF-8](https://www.rfc-editor.org/rfc/rfc3629)
- [RFC 8446: TLS 1.3](https://www.rfc-editor.org/rfc/rfc8446)
- [MDN: Content-Encoding](https://developer.mozilla.org/es/docs/Web/HTTP/Headers/Content-Encoding)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace la capa de presentación?
> Da formato a los datos para que ambos extremos los entiendan: codificación (UTF-8, ASCII), compresión y cifrado.

> [!question]- ¿Qué ejemplos de formatos se asocian a esta capa?
> JPEG, PNG, MP4, codificación de caracteres y el cifrado/descifrado que hace TLS.

