---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Carrera y liderazgo técnico
herramienta: Markdown, Mermaid
tags: [carrera-liderazgo, laboratorio]
---
# Lab 2 - Escribir un design doc corto

Parte de: [[14-Carrera-Liderazgo/02 Liderazgo y comunicación/_Índice Liderazgo y comunicación|Liderazgo y comunicación]]

**Cubre:** [[Cómo documentar una decisión técnica]] · [[Cómo explicar un tema técnico]] · [[Comunicación con personas no técnicas]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Escribir un design doc de 1–2 páginas para un cambio real o realista, y obtener comentarios antes de implementarlo.

## Pasos
1. Elige un cambio no trivial (p. ej. "notificaciones por correo cuando se publica un portafolio" o "agregar una segunda pasarela de pago").
2. Escribe las secciones: Contexto y problema, Objetivos, **No objetivos**, Propuesta (con diagrama Mermaid), Alternativas consideradas, Riesgos y mitigaciones, Plan de entrega por fases, Preguntas abiertas.
3. Agrega un resumen de 3 líneas al inicio, entendible por alguien no técnico.
4. Compártelo con al menos una persona (o léelo en voz alta tras un día) y pide comentarios en el documento.
5. Responde cada comentario y actualiza; registra la decisión final como ADR si aplica.

## Criterios de terminado
- [ ] Cabe en 2 páginas y tiene todas las secciones.
- [ ] Hay al menos 2 no objetivos y 2 alternativas con motivo de descarte.
- [ ] El resumen inicial lo entiende una persona no técnica (lo comprobaste).
- [ ] El diagrama renderiza y muestra el flujo principal.
- [ ] Recibiste y respondiste al menos 2 comentarios.

## Solución

> [!success]- Esqueleto de design doc
> ```markdown
> # Notificaciones al publicar un portafolio
> **Resumen:** Cuando un creador publique su portafolio, sus seguidores recibirán un correo.
> Se enviará en segundo plano para no hacer más lenta la publicación.
>
> ## Contexto y problema
> ## Objetivos
> - Correo a seguidores en < 10 min tras publicar.
> ## No objetivos
> - Notificaciones push o SMS.
> - Personalización avanzada de plantillas.
> ## Propuesta
> (evento PortafolioPublicado → outbox → worker → proveedor de correo; diagrama)
> ## Alternativas
> - Enviar en la misma petición: simple, pero lento y frágil si el proveedor falla.
> - Servicio de notificaciones aparte: excesivo para el volumen actual.
> ## Riesgos y mitigaciones
> ## Plan por fases
> ## Preguntas abiertas
> ```

## Notas personales

_Qué me costó, errores que cometí, dudas._
