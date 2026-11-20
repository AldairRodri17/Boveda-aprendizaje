---
proyecto: Conocimiento
tipo: laboratorio
nivel: avanzado
progreso: pendiente
estado: borrador
area: Arquitectura y patrones
herramienta: Papel o Excalidraw, Mermaid, .NET SDK
tags: [arquitectura-patrones, laboratorio]
---
# Lab 4 - Diseño e implementación de un rate limiter

Parte de: [[09-Arquitectura-Patrones/05 Diseño de sistemas/_Índice Diseño de sistemas|Diseño de sistemas]]

**Cubre:** [[Caso - rate limiter]] · [[Diseño de sistemas - cómo abordarlo]] · [[Requisitos funcionales y no funcionales]] · [[Estimaciones rápidas (carga, almacenamiento y costo)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Hacer un diseño de sistemas completo de un rate limiter (como en una entrevista) y luego implementar el algoritmo token bucket con pruebas.

## Pasos
1. Pon un cronómetro de 40 min. Escribe requisitos funcionales y no funcionales para: API pública, 10 M usuarios, 100 req/min por API key con ráfaga de 20, 30 instancias de API.
2. Haz las estimaciones: memoria del estado, operaciones por segundo contra el almacén compartido.
3. Elige algoritmo (token bucket, ventana fija, ventana deslizante) y justifica en 3 líneas.
4. Dibuja el diagrama en Mermaid (cliente, gateway/middleware, almacén, reglas) y escribe qué pasa si el almacén cae.
5. Implementa en C# una clase `TokenBucket(capacidad, tokensPorSegundo, reloj)` con `bool TryConsume()`; el reloj debe ser inyectable (`TimeProvider`).
6. Escribe pruebas: ráfaga de 20 permitida, la petición 21 rechazada, y tras 0.6 s se permite una más.
7. Integra un middleware que use un `TokenBucket` por API key (diccionario concurrente) y responda 429 con `Retry-After`.
8. Compara tu diseño con la solución y anota diferencias.

## Criterios de terminado
- [ ] El documento tiene requisitos, estimaciones con números, diagrama, decisión de algoritmo y comportamiento ante falla del almacén.
- [ ] Las tres pruebas del token bucket pasan sin `Thread.Sleep` (usando un reloj falso).
- [ ] 25 peticiones seguidas con la misma API key devuelven 20 respuestas 200 y 5 respuestas 429 con `Retry-After`.
- [ ] Puedes explicar por qué el limitador en memoria no basta con 30 instancias.

## Solución

> [!success]- Diseño de referencia
> Ver el ejercicio resuelto en [[Caso - rate limiter]]. Puntos clave: estado ≈ 500 MB en Redis, script atómico, 429 + `Retry-After`, *fail open* con alerta.

> [!success]- Token bucket con reloj inyectable
> ```csharp
> public sealed class TokenBucket(int capacidad, double tokensPorSegundo, TimeProvider reloj)
> {
>     private double _tokens = capacidad;
>     private long _ultimo = reloj.GetTimestamp();
>     private readonly object _lock = new();
>
>     public bool TryConsume()
>     {
>         lock (_lock)
>         {
>             var ahora = reloj.GetTimestamp();
>             var seg = reloj.GetElapsedTime(_ultimo, ahora).TotalSeconds;
>             _tokens = Math.Min(capacidad, _tokens + seg * tokensPorSegundo);
>             _ultimo = ahora;
>             if (_tokens < 1) return false;
>             _tokens -= 1;
>             return true;
>         }
>     }
>     public TimeSpan EsperaParaUno() =>
>         TimeSpan.FromSeconds(Math.Max(0, (1 - _tokens) / tokensPorSegundo));
> }
> ```
> 100/min ≈ 1.667 tokens/s → un token cada 0.6 s.

> [!success]- Pruebas con reloj falso
> ```csharp
> // Paquete Microsoft.Extensions.TimeProvider.Testing (FakeTimeProvider)
> [Fact]
> public void Rafaga_y_recarga()
> {
>     var reloj = new FakeTimeProvider();
>     var b = new TokenBucket(20, 100.0 / 60, reloj);
>     for (var i = 0; i < 20; i++) Assert.True(b.TryConsume());
>     Assert.False(b.TryConsume());
>     reloj.Advance(TimeSpan.FromMilliseconds(600));
>     Assert.True(b.TryConsume());
> }
> ```

> [!success]- Por qué no basta en memoria
> Cada instancia tendría su propio contador: con 30 instancias y balanceo, una key podría hacer hasta 30 × 100 req/min. Se necesita estado compartido (Redis) o dividir el límite por instancia aceptando imprecisión. Nota: ASP.NET Core trae `AddRateLimiter` con `TokenBucketRateLimiter`, pero es por instancia.

## Notas personales

_Qué me costó, errores que cometí, dudas._
