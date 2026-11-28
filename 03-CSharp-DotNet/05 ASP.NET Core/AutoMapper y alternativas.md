---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: C# y .NET
tags: [csharp-dotnet]
contenido: completo
---
# AutoMapper y alternativas

Parte de: [[03-CSharp-DotNet/05 ASP.NET Core/_Índice ASP.NET Core|ASP.NET Core]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**AutoMapper** es una biblioteca que copia datos entre objetos por **convención**: si `Egreso` tiene `Monto` y `EgresoDto` también, lo asigna solo, sin que escribas `dto.Monto = e.Monto`. Se configuran perfiles (`CreateMap<Egreso, EgresoDto>()`) y se usa `mapper.Map<EgresoDto>(egreso)`. Fue muy popular para convertir entidades en DTOs.

En tus proyectos la regla es **mapear a mano** ([[Reglas Back - .NET]]). Con Dapper y SQL en archivos, además, la consulta ya regresa directamente el DTO que necesita la pantalla, así que muchas veces ni siquiera hay mapeo.

### Cómo funciona
AutoMapper, al configurarse, recorre por **reflexión** las propiedades de origen y destino, empareja por nombre (también "aplanando": `Caja.Nombre` → `CajaNombre`), y genera en ejecución el código de copia. Lo que no puede emparejar lo configuras con `ForMember`. Si no llamas `AssertConfigurationIsValid()` en una prueba, una propiedad sin mapear simplemente queda en su valor por defecto.

**¿Por qué mapear a mano?**
- **Se ve**: abres el método y sabes de dónde sale cada valor.
- **Se depura**: puedes poner un breakpoint en la línea exacta.
- **El compilador avisa**: si renombras `Monto` a `Importe` en el DTO, el mapeo manual deja de compilar; con AutoMapper compila y el valor llega vacío **en ejecución**, a veces en producción.
- **Buscar referencias funciona**: "¿quién llena `CajaNombre`?" tiene respuesta en el IDE.
- **Sin dependencia ni licencia**.

El costo es escribir más líneas; en DTOs de 5 a 15 propiedades es poco, y con *records* y constructores posicionales queda compacto.

**Licencia.** Desde julio de 2025, AutoMapper **v15** usa una licencia dual comercial (Lucky Penny Software), igual que MediatR v13: edición comunitaria gratuita para organizaciones con ingresos anuales menores a 5 millones de USD, ONG bajo ese presupuesto, uso educativo y entornos no productivos, con llave de licencia; las versiones anteriores conservan su licencia original. Verifica los términos vigentes antes de usarlo con un cliente (Pendiente de confirmar condiciones actuales).

### Ejemplo
Mapeo manual con métodos de extensión, uno por par de tipos, junto al slice:

```csharp
public sealed record EgresoDto(long Id, decimal Monto, string Concepto, string Caja, DateTimeOffset Fecha);

public static class EgresoMapeos
{
    public static EgresoDto ToDto(this Egreso e) =>
        new(e.Id, e.Monto, e.Concepto, e.Caja.Nombre, e.Fecha);

    public static IReadOnlyList<EgresoDto> ToDto(this IEnumerable<Egreso> lista) =>
        lista.Select(ToDto).ToArray();
}
```

Si mañana agregas un parámetro al record `EgresoDto`, `ToDto` deja de compilar y te obliga a decidir de dónde sale el dato.

**Alternativa sin escribir todo: Mapperly** (`Riok.Mapperly`), un *source generator* gratuito y de código abierto. Declaras un método `partial` y Mapperly **genera el código de mapeo en compilación**, como si lo hubieras escrito tú: sin reflexión, rápido, compatible con Native AOT, y reporta como warning (que con `TreatWarningsAsErrors` es error) las propiedades que no pudo mapear.

```csharp
using Riok.Mapperly.Abstractions;

[Mapper]
public static partial class EgresoMapper
{
    [MapProperty("Caja.Nombre", nameof(EgresoDto.Caja))]   // ruta anidada del origen
    public static partial EgresoDto ToDto(Egreso e);
}
```

Puedes abrir el código generado en el IDE (Dependencias → Analizadores) y depurarlo.

Con **Dapper**, la mejor alternativa a mapear es no tener que hacerlo: el `SELECT` del archivo `.sql` regresa exactamente las columnas del DTO (`SELECT e.Id, e.Monto, e.Concepto, c.Nombre AS Caja, e.Fecha ...`) y Dapper llena el record directamente ([[Dapper]], [[SQL en archivos con una interfaz por consulta]]).

### Cuándo usarlo y cuándo no
- **Mapeo manual** por defecto: pocos tipos, reglas claras, máxima visibilidad.
- **Mapperly** cuando hay muchos DTOs grandes y repetitivos y no quieres escribir cada línea, sin renunciar a la verificación en compilación.
- **AutoMapper** si un proyecto existente ya lo usa (y la licencia aplica a tu caso); al menos agrega una prueba con `AssertConfigurationIsValid()`.
- **Proyección en SQL** con Dapper siempre que la consulta pueda regresar el DTO final.

### Errores comunes
- **Lógica de negocio dentro de perfiles** de AutoMapper (`ForMember` con cálculos): queda escondida.
- **Sin prueba de configuración**: propiedades que llegan vacías sin aviso.
- **Mapear entidades completas** solo para mostrar tres columnas: trae de la BD solo lo necesario.
- **Mapeos "genéricos" con reflexión caseros**: los mismos problemas de AutoMapper sin su madurez.
- **Actualizar a AutoMapper v15** sin revisar la licencia.

### Para profundizar
- [Mapperly: documentación oficial](https://mapperly.riok.app/)
- [Anuncio de las ediciones comerciales de AutoMapper y MediatR (Jimmy Bogard)](https://www.jimmybogard.com/automapper-and-mediatr-commercial-editions-launch-today/)
- [AutoMapper: sitio oficial](https://automapper.io/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Por qué tu regla es mapear a mano?
> El mapeo manual se ve, se depura y el compilador avisa cuando cambia un tipo; AutoMapper oculta errores hasta ejecutar y complica rastrear de dónde sale un valor.

> [!question]- ¿Qué cambió en la licencia de AutoMapper?
> Igual que MediatR, desde 2025 pasó a licencia comercial con edición comunitaria (Pendiente de confirmar condiciones actuales).

> [!question]- ¿Qué alternativas hay si no quieres escribir todo?
> Mapperly (source generator: genera el código de mapeo en compilación, sin reflexión) o métodos de extensión `ToDto()` escritos a mano.

