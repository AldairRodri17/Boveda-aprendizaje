---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Seguridad
tags: [seguridad]
---
# Cifrado asimétrico (RSA y ECC)

Parte de: [[Cifrado simétrico y asimétrico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El **cifrado asimétrico** (o de llave pública) usa un **par de llaves** matemáticamente relacionadas: una **pública**, que puedes repartir a todo el mundo, y una **privada**, que solo tiene su dueño. Lo que se hace con una solo se deshace con la otra. Los dos algoritmos que verás en la práctica son **RSA** y la familia de **curvas elípticas (ECC)**: ECDSA y Ed25519 para firmar, ECDH para acordar llaves.

### Cómo funciona
Hay dos usos y conviene no mezclarlos:

| Uso | Quién usa la llave pública | Quién usa la privada |
|---|---|---|
| **Cifrar** (confidencialidad) | Cualquiera la usa para cifrar un mensaje dirigido a ti | Solo tú la usas para descifrarlo |
| **Firmar** (autenticidad e integridad) | Cualquiera la usa para verificar la firma | Solo tú la usas para firmar |

Así funciona tu Xolotl.Hecate con JWT RS256: el servidor de identidad **firma** los tokens con la llave privada, y cada API los **verifica** con la llave pública que descarga del endpoint JWKS. Las APIs nunca necesitan la privada, así que si una API se ve comprometida no puede emitir tokens.

**Por qué no se cifran archivos grandes con RSA**: RSA es lento (miles de veces más que AES) y además solo puede cifrar un bloque menor que el tamaño de la llave. Con una llave de 2048 bits y relleno OAEP con SHA-256 el máximo es de 190 bytes. Por eso se usa **cifrado híbrido**: generas una llave AES aleatoria, cifras el archivo con AES-GCM y cifras solo esa llave (32 bytes) con RSA. Es lo que hacen TLS, PGP y los servicios de llaves en la nube.

**Ventaja de ECC**: ofrece la misma seguridad con llaves mucho más pequeñas. Una curva P-256 da una seguridad comparable a RSA de 3072 bits. Eso significa firmas y llaves más cortas, operaciones más rápidas y menos ancho de banda, algo útil en tokens, certificados y dispositivos móviles. RSA sigue siendo muy compatible y es perfectamente válido con 2048 bits o más.

### Ejemplo
Firmar y verificar con ECDSA en .NET 10:

```csharp
using System.Security.Cryptography;
using System.Text;

using var ecdsa = ECDsa.Create(ECCurve.NamedCurves.nistP256);
byte[] datos = Encoding.UTF8.GetBytes("{\"pedido\":1024,\"total\":350.00}");

// Firma con la llave privada
byte[] firma = ecdsa.SignData(datos, HashAlgorithmName.SHA256);

// Exporta solo la pública para quien va a verificar
byte[] publica = ecdsa.ExportSubjectPublicKeyInfo();

using var verificador = ECDsa.Create();
verificador.ImportSubjectPublicKeyInfo(publica, out _);
bool valida = verificador.VerifyData(datos, firma, HashAlgorithmName.SHA256);
Console.WriteLine(valida); // True; si cambias un byte de "datos", False
```

Cifrar una llave pequeña con RSA-OAEP:

```csharp
using var rsa = RSA.Create(3072);
byte[] llaveAes = RandomNumberGenerator.GetBytes(32);
byte[] llaveCifrada = rsa.Encrypt(llaveAes, RSAEncryptionPadding.OaepSHA256);
byte[] recuperada = rsa.Decrypt(llaveCifrada, RSAEncryptionPadding.OaepSHA256);
```

En producción la llave privada no vive en un archivo del proyecto: va en Azure Key Vault (idealmente como llave no exportable) o en un certificado administrado.

### Cuándo usarlo y cuándo no
- **Úsalo** para firmas (JWT RS256/ES256, firmas de paquetes, documentos), intercambio de llaves y para cifrar llaves simétricas.
- **No lo uses** para cifrar datos voluminosos directamente; combina con AES.
- Entre RSA y ECC: ECC para sistemas nuevos cuando todos los participantes lo soportan; RSA cuando necesitas máxima compatibilidad.

### Errores comunes
- **Usar relleno PKCS#1 v1.5 para cifrar.** Tiene ataques conocidos; usa OAEP para cifrar y PSS (o PKCS#1 v1.5 solo en firmas por compatibilidad) para firmar.
- **Llaves RSA de 1024 bits.** Ya no se consideran seguras; mínimo 2048.
- **Confundir los roles**: "cifrar con la privada" no es una forma de confidencialidad, porque cualquiera con la pública lo descifra. Eso es firmar.
- **Guardar la llave privada en el repositorio** junto al código. Ver [[Manejo de secretos]].

### Para profundizar
- [Modelo de criptografía en .NET (Microsoft Learn)](https://learn.microsoft.com/en-us/dotnet/standard/security/cryptography-model)
- [Clase ECDsa](https://learn.microsoft.com/en-us/dotnet/api/system.security.cryptography.ecdsa)
- [Clase RSA](https://learn.microsoft.com/en-us/dotnet/api/system.security.cryptography.rsa)
- [OWASP Cryptographic Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Para qué se usa la llave pública y para qué la privada?
> Cifrar: con la pública, y solo la privada descifra. Firmar: con la privada, y cualquiera verifica con la pública.

> [!question]- ¿Por qué no se cifran archivos grandes directamente con RSA?
> RSA es lento y solo cifra datos pequeños (menores al tamaño de la llave). Se cifra el archivo con AES y la llave AES con RSA (cifrado híbrido).

> [!question]- ¿Qué ventaja tienen las curvas elípticas (ECC) frente a RSA?
> Ofrecen seguridad comparable con llaves mucho más cortas, lo que da firmas e intercambios más rápidos y pequeños.

