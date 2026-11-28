---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Seguridad
herramienta: draw.io o Microsoft Threat Modeling Tool
tags: [seguridad, laboratorio]
contenido: completo
---
# Lab 2 - Modelado de amenazas STRIDE de una app propia

Parte de: [[12-Seguridad/01 Fundamentos de seguridad/_Índice Fundamentos de seguridad|Fundamentos de seguridad]]

**Cubre:** [[Mentalidad de seguridad y modelado de amenazas]] · [[Superficie de ataque]] · [[Defensa en profundidad]] · [[Principio de mínimo privilegio]] · [[Tríada CIA (confidencialidad, integridad y disponibilidad)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Hacer el modelo de amenazas de una app tuya (por ejemplo una SPA en React + API .NET + SQL Server + Blob Storage) y salir con una lista priorizada de mitigaciones.

## Pasos
1. Dibuja el diagrama de flujo de datos (DFD): actores externos, procesos (SPA, API), almacenes (SQL, Blob) y flujos. Marca los **límites de confianza** (internet ↔ API, API ↔ base).
2. Lista los activos y clasifícalos con CIA (¿qué pasa si se filtra, se altera o no está disponible?).
3. Recorre cada flujo que cruza un límite y aplica STRIDE: escribe al menos una amenaza por letra.
4. Para cada amenaza, anota el control existente o el que falta, y una prioridad (alta, media, baja).
5. Identifica la superficie de ataque: endpoints públicos, puertos, paneles, integraciones.
6. Revisa los permisos de cada identidad (usuario de BD, identidad de la app en Azure) con el principio de mínimo privilegio.

## Criterios de terminado
- [ ] El DFD tiene al menos 2 límites de confianza marcados.
- [ ] Hay una tabla con mínimo 6 amenazas (una por cada letra de STRIDE) y su mitigación.
- [ ] Cada activo tiene su impacto CIA anotado.
- [ ] Hay al menos 3 mitigaciones nuevas con dueño y prioridad.
- [ ] Al menos un control está en dos capas distintas (defensa en profundidad).

## Solución

> [!success]- Ejemplo de tabla STRIDE (API .NET + SQL)
> | STRIDE | Amenaza | Mitigación |
> |---|---|---|
> | Spoofing | Token robado reutilizado | Tokens cortos, HTTPS, refresh revocable |
> | Tampering | Cambiar `tenantId` en el cuerpo | Tomar el tenant del token en el servidor |
> | Repudiation | Usuario niega haber borrado un registro | Bitácora de auditoría con usuario y fecha |
> | Information disclosure | Error de SQL mostrado al cliente | ProblemDetails genérico + log interno |
> | Denial of service | Búsquedas sin paginar | Tamaño máximo de página + rate limiting |
> | Elevation of privilege | Campo `rol` aceptado en el JSON | DTOs de entrada sin campos sensibles |

> [!success]- Ejemplo de mínimo privilegio
> - Usuario SQL de la API: `EXECUTE` en el esquema `dbo`, sin `db_owner`.
> - Identidad administrada: *Storage Blob Data Contributor* solo sobre el contenedor de la app.
> - Las migraciones corren en el pipeline con otra identidad.

## Notas personales

_Qué me costó, errores que cometí, dudas._
