---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Power BI
herramienta: Power BI Desktop
tags: [power-bi, laboratorio]
contenido: completo
---
# Lab 5 - Seguridad a nivel de fila (RLS) en Power BI

Parte de: [[17-Power-BI/03 Modelado de datos/_Índice Modelado de datos|Modelado de datos]]

**Cubre:** [[Seguridad a nivel de fila (RLS)]] · [[DAX - seguridad dinámica con USERPRINCIPALNAME]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Crear un rol estático y uno dinámico basado en una tabla de permisos, y probarlos con *Ver como*.

## Pasos

1. Sobre el modelo del Lab 4, crea el rol `Solo Bebidas` con filtro en Productos.
2. Agrega una tabla `Permisos` (Correo, Categoria) con *Especificar datos*: `ana@contoso.com` → Bebidas, `ana@contoso.com` → Alimentos, `luis@contoso.com` → Equipo.
3. Crea el rol `Dinámico` que filtre Productos según las categorías del usuario actual (sin relación física, usando una expresión en Productos).
4. Usa *Modelado → Ver como* con `Solo Bebidas` y anota el total.
5. Usa *Ver como* con *Otro usuario* = `ana@contoso.com` y rol `Dinámico`; repite con `luis@contoso.com` y con un correo que no esté en la tabla.
6. Oculta la tabla Permisos.

## Criterios de terminado

- [ ] Con `Solo Bebidas` la tarjeta muestra 500.
- [ ] Con `ana@contoso.com` + Dinámico muestra 700 (Bebidas 500 + Alimentos 200).
- [ ] Con `luis@contoso.com` muestra 750.
- [ ] Con un correo no registrado no se ve ninguna venta.
- [ ] Sabes explicar por qué los miembros del workspace no quedan restringidos por estos roles.

## Solución

> [!success]- Filtro rol estático
> Tabla Productos:
> ```dax
> [Categoria] = "Bebidas"
> ```

> [!success]- Filtro rol dinámico
> Tabla Productos:
> ```dax
> [Categoria] IN
>     CALCULATETABLE(VALUES(Permisos[Categoria]), Permisos[Correo] = USERPRINCIPALNAME())
> ```
> Al no haber relación, la tabla Permisos no se filtra sola; la expresión lee sus valores para el usuario actual.

> [!success]- Recordatorio del Service
> Al publicar, asigna personas o grupos al rol en *Seguridad* del modelo semántico. Los roles de Administrador/Miembro/Colaborador del workspace ven todo.

## Notas personales

_Qué me costó, errores que cometí, dudas._
