---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Integraciones
herramienta: nslookup/dig, MXToolbox (gratis) y una cuenta de Gmail para ver los resultados de autenticación
tags: [integraciones, laboratorio]
contenido: completo
---
# Lab 2 - Verificar SPF, DKIM y DMARC de tu dominio

Parte de: [[22-Integraciones/02 Correo/_Índice Correo|Correo]]

**Cubre:** [[SPF, DKIM y DMARC]] · [[SPF (Sender Policy Framework)]] · [[DKIM (firma de correo)]] · [[DMARC (política y reportes)]] · [[Correo transaccional (SMTP y proveedores)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Revisar y, si hace falta, corregir la autenticación de correo de un dominio propio, comprobando con herramientas públicas que SPF, DKIM y DMARC pasan y están alineados.

## Pasos

1. Elige un dominio que controles (si no tienes, usa los de tu empresa solo con permiso y **sin cambiar nada**, en modo lectura).
2. Consulta el SPF: `nslookup -type=txt tudominio.com`. Cuenta los `include` y revisa que haya un solo `v=spf1`.
3. Envía un correo desde ese dominio a una cuenta de Gmail y abre *Mostrar original*. Anota el resultado de SPF, DKIM y DMARC y el selector `s=` y dominio `d=` de la firma DKIM.
4. Consulta la llave pública DKIM: `nslookup -type=txt SELECTOR._domainkey.tudominio.com`.
5. Consulta el DMARC: `nslookup -type=txt _dmarc.tudominio.com`.
6. Verifica todo en MXToolbox (SPF Record Lookup, DKIM Lookup y DMARC Lookup) y anota avisos.
7. Si no hay DMARC, publica `v=DMARC1; p=none; rua=mailto:...` en tu DNS (solo en un dominio propio).
8. Explica en Notas personales si DMARC pasa por SPF alineado, por DKIM alineado o por ambos.

## Criterios de terminado

- [ ] Tienes anotado el registro SPF completo y el número de búsquedas DNS (≤ 10).
- [ ] En *Mostrar original* de Gmail aparecen `SPF: PASS`, `DKIM: PASS` y `DMARC: PASS` (o anotaste cuál falla y por qué).
- [ ] La consulta a `SELECTOR._domainkey` devuelve un registro con `p=` (la llave pública).
- [ ] Existe un registro en `_dmarc.tudominio.com` con al menos `v=DMARC1` y `p=`.
- [ ] MXToolbox no reporta errores de sintaxis en los tres registros.

## Solución

> [!success]- Ejemplos de registros correctos
> ```text
> tudominio.com.           TXT "v=spf1 include:spf.protection.outlook.com include:_spf.proveedor.com -all"
> s1._domainkey.tudominio.com. TXT "v=DKIM1; k=rsa; p=MIIBIjANBgkqh..."
> _dmarc.tudominio.com.    TXT "v=DMARC1; p=none; rua=mailto:dmarc@tudominio.com"
> ```
> Los `include` exactos dependen de tus proveedores; cópialos de su documentación.

> [!success]- Cómo leer el resultado
> - **SPF PASS** pero el dominio del Return-Path es del proveedor → SPF no está alineado.
> - **DKIM PASS** con `d=tudominio.com` → DKIM alineado: con esto DMARC pasa.
> - Si DKIM firma con `d=proveedor.com`, configura en el proveedor la firma con tu dominio (te pedirá publicar CNAME o TXT de `_domainkey`).
> - `permerror` en SPF suele ser por dos registros `v=spf1` o más de 10 búsquedas DNS.

## Notas personales

_Qué me costó, errores que cometí, dudas._
