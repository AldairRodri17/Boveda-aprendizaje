---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Arquitectura y patrones
herramienta: VS Code, Markdown y Mermaid
tags: [arquitectura-patrones, laboratorio]
contenido: completo
---
# Lab 5 - Escribir un ADR real en ARQUITECTURA.md

Parte de: [[09-Arquitectura-Patrones/06 Documentar decisiones/_Índice Documentar decisiones|Documentar decisiones]]

**Cubre:** [[ADR - registro de decisiones de arquitectura]] · [[Design docs y RFCs]] · [[Diagramas C4]] · [[C4 nivel 1 - Contexto]] · [[C4 nivel 2 - Contenedores]] · [[Diagramas de secuencia]] · [[Cómo elegir una arquitectura]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Escribir un ADR real sobre una decisión de uno de tus proyectos y agregarlo a su `ARQUITECTURA.md`, con diagrama de contexto, de contenedores y de secuencia.

## Pasos
1. Elige una decisión real ya tomada en un proyecto (p. ej. estrategia de multi-tenancy con filtros globales, mediador propio en lugar de una librería, outbox para eventos, Strategy para pasarelas de pago).
2. Revisa el `ARQUITECTURA.md` del proyecto: busca la numeración de ADRs existente y el formato que ya usas.
3. Escribe el ADR con: título en forma de decisión, fecha, estado, contexto (fuerzas y restricciones), decisión, consecuencias positivas y negativas, alternativas consideradas con el motivo de descarte.
4. Agrega un diagrama C4 de contexto y uno de contenedores en Mermaid (flowchart con etiquetas de tecnología en cada caja y en cada flecha).
5. Agrega un diagrama de secuencia del flujo principal afectado por la decisión.
6. Pide a alguien (o a tu yo de mañana) que lea solo el ADR y responda: ¿por qué no se eligió la alternativa B? Si no puede, mejora el contexto.
7. Haz commit con un mensaje que referencie el ADR.

## Criterios de terminado
- [ ] El ADR tiene número, fecha y estado, y ocupa menos de una página.
- [ ] Lista al menos 2 alternativas con el motivo concreto de descarte.
- [ ] Las consecuencias incluyen al menos una negativa aceptada explícitamente.
- [ ] Los tres diagramas renderizan en Obsidian / GitHub sin errores de Mermaid.
- [ ] Un lector externo puede explicar la decisión y su porqué solo con el documento.

## Solución

> [!success]- Plantilla de ADR
> ```markdown
> ## ADR-007: Aislar tenants con filtros globales de EF Core
> - Fecha: 2026-11-09
> - Estado: Aceptado
>
> ### Contexto
> Un SaaS con cientos de creadores en una sola base. Un WHERE olvidado expone datos
> de otro tenant. Equipo de 1–2 personas; presupuesto bajo.
>
> ### Decisión
> Tabla compartida con PortfolioId; el tenant se resuelve en un middleware desde el
> token y se aplica con HasQueryFilter en todas las entidades con tenant.
>
> ### Consecuencias
> + El código de negocio no repite el filtro; un solo punto de revisión.
> - IgnoreQueryFilters() es una puerta trasera: solo en casos listados y probados.
> - Vecino ruidoso posible; se monitorea consumo por tenant.
>
> ### Alternativas
> - Base por tenant: aislamiento máximo, costo y operación inviables hoy.
> - Row-Level Security: buena defensa extra; se pospone por complejidad de pruebas.
> ```

> [!success]- Diagramas de ejemplo
> ```mermaid
> flowchart LR
>   U[Creador - persona] -->|HTTPS| S[Sistema de portafolios]
>   V[Visitante] -->|HTTPS| S
>   S -->|OIDC| IdP[Proveedor de identidad]
>   S -->|SMTP| M[Servicio de correo]
> ```
> ```mermaid
> flowchart LR
>   SPA[SPA - React] -->|JSON/HTTPS| API[API - ASP.NET Core]
>   API -->|EF Core| DB[(SQL Server)]
>   API -->|Outbox| W[Worker - .NET]
>   W -->|AMQP| Q[[Broker]]
> ```
> ```mermaid
> sequenceDiagram
>   participant C as Cliente
>   participant M as TenantMiddleware
>   participant H as Handler
>   participant DB as DbContext
>   C->>M: Bearer token
>   M->>M: resolver tenant desde sub
>   M->>H: petición con TenantContext
>   H->>DB: consulta
>   DB->>DB: agrega WHERE PortfolioId = @tenant
> ```

## Notas personales

_Qué me costó, errores que cometí, dudas._
