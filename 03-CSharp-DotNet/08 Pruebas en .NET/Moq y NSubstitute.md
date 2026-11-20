---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: C# y .NET
tags: [csharp-dotnet]
---
# Moq y NSubstitute

Parte de: [[03-CSharp-DotNet/08 Pruebas en .NET/_Índice Pruebas en .NET|Pruebas en .NET]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Moq y NSubstitute son las dos bibliotecas más populares en .NET para crear **dobles de prueba** dinámicos (stubs y mocks) sin escribir clases a mano. Generan en tiempo de ejecución una implementación de una interfaz (o de una clase con miembros virtuales), te dejan configurar qué devuelve cada método y luego verificar qué llamadas recibió. El concepto detrás está en [[Mocks, stubs y fakes]].

### Cómo funciona
Ambas usan proxies dinámicos (Castle DynamicProxy): crean una clase en memoria que implementa la interfaz y registra cada llamada. La diferencia es el estilo de la API:

- **Moq** envuelve el doble en un objeto `Mock<T>`; configuras con expresiones lambda (`Setup`, `Returns`, `Verify`) y obtienes la instancia con `.Object`.
- **NSubstitute** te da directamente la instancia (`Substitute.For<T>()`) y configuras llamando al método como si fuera real (`.Returns(...)`), con una sintaxis más corta.

| Tarea | Moq | NSubstitute |
|---|---|---|
| Crear | `var m = new Mock<IObtenerCliente>();` | `var s = Substitute.For<IObtenerCliente>();` |
| Stub | `m.Setup(x => x.EjecutarAsync(5, It.IsAny<CancellationToken>())).ReturnsAsync(cli);` | `s.EjecutarAsync(5, Arg.Any<CancellationToken>()).Returns(cli);` |
| Cualquier argumento | `It.IsAny<int>()` | `Arg.Any<int>()` |
| Argumento con condición | `It.Is<int>(i => i > 0)` | `Arg.Is<int>(i => i > 0)` |
| Lanzar excepción | `.ThrowsAsync(new TimeoutException())` | `.Throws(new TimeoutException())` (o `ThrowsAsync` con el paquete de extensiones) |
| Usar la instancia | `m.Object` | `s` |
| Verificar llamada | `m.Verify(x => x.EnviarAsync("a@b.mx", It.IsAny<string>(), It.IsAny<CancellationToken>()), Times.Once());` | `await s.Received(1).EnviarAsync("a@b.mx", Arg.Any<string>(), Arg.Any<CancellationToken>());` |
| Verificar que no hubo llamada | `Times.Never()` | `DidNotReceive()` |

### Ejemplo
La misma prueba de un handler de Itzamna.Dionisio con cada biblioteca:

```csharp
// Moq
[Fact]
public async Task No_notifica_si_el_cliente_no_existe_Moq()
{
    var clientes = new Mock<IObtenerCliente>();
    clientes.Setup(x => x.EjecutarAsync(It.IsAny<int>(), It.IsAny<CancellationToken>()))
            .ReturnsAsync((ClienteDto?)null);
    var notif = new Mock<INotificador>();
    var h = new CrearPedidoHandler(clientes.Object, Mock.Of<IGuardarPedido>(), notif.Object);

    var r = await h.Handle(new CrearPedido(99, 100m), CancellationToken.None);

    Assert.Equal("CLIENTE_NO_EXISTE", r.Codigo);
    notif.Verify(x => x.EnviarAsync(It.IsAny<string>(), It.IsAny<string>(), It.IsAny<CancellationToken>()), Times.Never());
}

// NSubstitute
[Fact]
public async Task No_notifica_si_el_cliente_no_existe_NSubstitute()
{
    var clientes = Substitute.For<IObtenerCliente>();
    clientes.EjecutarAsync(Arg.Any<int>(), Arg.Any<CancellationToken>()).Returns((ClienteDto?)null);
    var notif = Substitute.For<INotificador>();
    var h = new CrearPedidoHandler(clientes, Substitute.For<IGuardarPedido>(), notif);

    var r = await h.Handle(new CrearPedido(99, 100m), CancellationToken.None);

    Assert.Equal("CLIENTE_NO_EXISTE", r.Codigo);
    await notif.DidNotReceive().EnviarAsync(Arg.Any<string>(), Arg.Any<string>(), Arg.Any<CancellationToken>());
}
```

Por defecto ambos son "sueltos": un método no configurado devuelve el valor por defecto (`null`, `0`, una `Task` completada). Moq ofrece `MockBehavior.Strict` para fallar ante llamadas no configuradas; úsalo con moderación porque vuelve las pruebas frágiles.

### La polémica de Moq en 2023
En agosto de 2023, la versión **4.20.0** de Moq incluyó **SponsorLink**, un componente del autor que durante la compilación leía el correo configurado en `git` del desarrollador, lo convertía en un hash y lo enviaba a un servicio externo para comprobar si era patrocinador del proyecto. Se hizo sin aviso claro ni consentimiento, lo que la comunidad consideró una violación de privacidad (y un problema potencial de cumplimiento, por ejemplo con GDPR). La versión fue retirada de NuGet en menos de un día y el componente se quitó en versiones posteriores, pero muchos equipos migraron a NSubstitute o fijaron versiones. La lección práctica: **revisa lo que traen tus dependencias**, fija versiones, usa herramientas de análisis de paquetes en Azure DevOps y evalúa la confianza en el mantenedor, no solo las funciones.

### Cuándo usarlo y cuándo no
Úsalas cuando un doble manual sería repetitivo (muchas variantes de respuesta) o necesitas verificar llamadas con argumentos complejos. Si tus interfaces son pequeñas, un fake manual puede ser más legible y no depende de terceros. Elegir entre ambas es cuestión de estilo y de política del equipo; NSubstitute suele leerse más natural, Moq tiene más historia y ejemplos.

### Errores comunes
- **Olvidar `.Object`** en Moq y pasar el `Mock<T>`: no compila o inyectas lo que no es.
- **Configurar con valores exactos** y luego llamar con otros: el stub devuelve `null` y la prueba falla de forma confusa.
- **Verificar todo**: pruebas atadas a la implementación.
- **Mockear clases concretas sin miembros virtuales**: el proxy no puede interceptarlas; trabaja con interfaces.
- **No esperar (`await`) `Received()`** sobre métodos async en NSubstitute: el analizador te avisa; respétalo.

### Para profundizar
- [Moq en GitHub](https://github.com/devlooped/moq)
- [Documentación de NSubstitute](https://nsubstitute.github.io/help/getting-started/)
- [Reporte de Snyk sobre Moq 4.20.0 (2023)](https://snyk.io/blog/moq-package-exfiltrates-user-emails/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo se hace un stub en cada una?
> Moq: `var m = new Mock<IReloj>(); m.Setup(x => x.Hoy()).Returns(fecha); m.Object`. NSubstitute: `var s = Substitute.For<IReloj>(); s.Hoy().Returns(fecha);`.

> [!question]- ¿Cómo verificas una llamada?
> Moq: `m.Verify(x => x.Enviar(It.IsAny<Correo>()), Times.Once);`. NSubstitute: `s.Received(1).Enviar(Arg.Any<Correo>());`.

> [!question]- ¿Qué polémica tuvo Moq en 2023?
> La versión 4.20 incluyó SponsorLink, que leía el correo configurado en git del desarrollador y enviaba un hash; se retiró tras las quejas y muchos equipos migraron a NSubstitute.

## Práctica

- [[Lab 10 - Pruebas con xUnit y WebApplicationFactory]]

