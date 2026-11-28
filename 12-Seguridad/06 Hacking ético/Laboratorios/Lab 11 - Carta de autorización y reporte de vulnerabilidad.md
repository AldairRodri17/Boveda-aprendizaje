---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Seguridad
herramienta: Obsidian o editor de texto, calculadora CVSS de FIRST
tags: [seguridad, laboratorio]
contenido: completo
---
# Lab 11 - Carta de autorización y reporte de vulnerabilidad

Parte de: [[12-Seguridad/06 Hacking ético/_Índice Hacking ético|Hacking ético]]

**Cubre:** [[Marco legal y autorización por escrito]] · [[Alcance (scope) de una prueba de seguridad]] · [[Cómo escribir un reporte de vulnerabilidad]] · [[CVE y CVSS]] · [[Divulgación responsable]] · [[Qué nunca hacer]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Redactar la carta de autorización y alcance para una prueba ficticia sobre una app tuya, y escribir un reporte profesional de un hallazgo real que hayas encontrado en Lab 3, 7 u 8 (en tu propio entorno).

## Pasos
1. Escribe una carta de autorización con: partes, sistemas en alcance y fuera de alcance, fechas, técnicas permitidas y prohibidas, manejo de datos, contacto de emergencia y firmas.
2. Toma un hallazgo de tus labs anteriores (por ejemplo el endpoint con IDOR o el JWT sin validar `exp`).
3. Calcula su CVSS en la calculadora oficial de FIRST y guarda el vector.
4. Escribe el reporte: título, resumen, activo, gravedad con vector, pasos para reproducir (con tus usuarios de prueba), impacto, evidencia, recomendación y referencias (CWE y OWASP).
5. Escribe un `security.txt` de ejemplo para tu app.
6. Pide a alguien (o a Claude) que intente reproducir el hallazgo solo con tu reporte.

## Criterios de terminado
- [ ] La carta tiene alcance incluido **y** excluido, fechas, técnicas prohibidas y contacto de emergencia.
- [ ] El reporte trae un vector CVSS completo y coincide con la puntuación.
- [ ] Los pasos para reproducir se pueden seguir sin preguntarte nada.
- [ ] La recomendación es concreta (qué cambiar en qué capa).
- [ ] El `security.txt` tiene al menos `Contact` y `Expires`.

## Solución

> [!success]- Plantilla de reporte
> ```markdown
> ## [Alta] IDOR en GET /api/pedidos/{id} permite ver pedidos de otros clientes
> **Activo:** api.lab.local (entorno propio de pruebas)
> **CVSS 3.1:** 6.5 — AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N
> **CWE:** CWE-639 (Authorization Bypass Through User-Controlled Key)
>
> ### Resumen
> El endpoint devuelve cualquier pedido por id sin comprobar que pertenece al usuario autenticado.
>
> ### Pasos para reproducir
> 1. Inicia sesión como `usuario_a` y crea un pedido; anota su id.
> 2. Inicia sesión como `usuario_b`.
> 3. Pide `GET /api/pedidos/<id de usuario_a>` con el token de `usuario_b`.
> 4. La respuesta es 200 con los datos del pedido de `usuario_a`.
>
> ### Impacto
> Cualquier cliente autenticado puede leer pedidos (nombre, dirección, productos) de otros clientes.
>
> ### Recomendación
> Filtrar por `UsuarioId` tomado del token en la consulta y responder 404 si no coincide. Agregar una prueba de integración con dos usuarios.
>
> ### Referencias
> OWASP API Security Top 10 2023 – API1 (BOLA); OWASP Top 10 2021 – A01.
> ```

> [!success]- security.txt de ejemplo
> ```text
> Contact: mailto:seguridad@midominio.com
> Expires: 2027-12-31T23:59:59Z
> Preferred-Languages: es, en
> Policy: https://midominio.com/seguridad/politica
> ```
> Se publica en `/.well-known/security.txt` (RFC 9116).

## Notas personales

_Qué me costó, errores que cometí, dudas._
