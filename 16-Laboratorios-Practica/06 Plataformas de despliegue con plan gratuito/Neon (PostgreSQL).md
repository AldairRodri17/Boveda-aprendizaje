---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Laboratorios y práctica
tags: [laboratorios-practica]
---
# Neon (PostgreSQL)

Parte de: [[16-Laboratorios-Practica/06 Plataformas de despliegue con plan gratuito/_Índice Plataformas de despliegue con plan gratuito|Plataformas de despliegue con plan gratuito]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Neon es un **PostgreSQL serverless**: una base de datos Postgres gestionada que **separa el almacenamiento del cómputo**, **escala a cero** cuando no hay uso (para no cobrar por tiempo ocioso) y permite crear **ramas (branches)** de la base como si fueran ramas de Git. Es una opción moderna y muy cómoda para tener Postgres gratis en proyectos de práctica, sobre todo si quieres entornos de prueba por rama.

### Qué se practica ahí
Trabajar con Postgres real sin instalar nada, practicar SQL y migraciones, y experimentar con el **branching** de bases: crear una copia de los datos por pull request o por entorno. Es un buen compañero de [[PostgreSQL]] y de proyectos como Pochteca.Nomisma.

### Cómo funciona
```mermaid
flowchart LR
  C[Cómputo: escala a cero] --- S[(Almacenamiento compartido)]
  S --> B1[rama main]
  S --> B2[rama pr-123: copia instantánea]
```

Como el almacenamiento está separado, crear una rama no duplica todos los datos: es casi instantáneo y barato.

### Para qué sirven las ramas de base de datos
Para tener una **copia de la base por pull request o por entorno de pruebas**, creada en segundos sin duplicar todo el almacenamiento. Pruebas una migración en la rama `pr-123`, verificas que no rompe nada y la descartas; tu rama `main` queda intacta. Es como `git branch`, pero para los datos.

### Qué cuidados tiene el plan gratuito
Según la documentación vigente (verificar en la página oficial): hasta **100 proyectos**, **1 GB** de almacenamiento por proyecto (20 GB en total), **100 CU-horas de cómputo** por proyecto al mes (unas 400 horas de un cómputo de 0.25 CU), autoescalado hasta 2 CU, hasta **10 ramas por proyecto**, un cómputo que **escala a cero tras 5 minutos** de inactividad (no se puede desactivar) (la primera consulta tras dormir tiene una pequeña **latencia de arranque**) y una ventana de restauración de hasta 6 horas. Si pasas el cómputo, el servicio se suspende hasta el siguiente periodo; si pasas el almacenamiento, se bloquean las escrituras que lo aumenten, pero no se borran datos. Usa la cadena de conexión con `sslmode=require` y guárdala como **secreto** ([[Secretos en repositorios]]).

### Cómo empezar paso a paso
1. Crea una cuenta y un proyecto en Neon; elige región.
2. Copia la **cadena de conexión** (incluye `sslmode=require`) y guárdala como secreto.
3. Conéctate con `psql` o desde tu app:

```bash
# Conectarse a Neon con SSL obligatorio
psql "postgresql://usuario:clave@host.neon.tech/basedatos?sslmode=require"
```

4. Crea una **rama** para probar una migración sin tocar `main`.
5. Vigila el consumo de cómputo y almacenamiento en el panel.

### Ejemplo
Para una API .NET que use Dapper contra Postgres, apuntas la cadena de conexión de Neon en una variable de entorno. En CI podrías crear una rama de Neon por pull request, correr las pruebas de integración contra esa copia y borrarla al cerrar el PR.

### Cuándo usarlo y cuándo no
Úsalo cuando quieras Postgres gratis con branching y escala a cero, ideal para entornos de prueba por PR. Si necesitas además autenticación, almacenamiento de archivos y API automática, [[Supabase]] da más de una vez. Para datos que deban persistir de verdad, revisa bien los límites del plan gratuito; para producción seria, un plan de pago o Azure.

### Errores comunes
- **No esperar la latencia de arranque.** Tras escalar a cero, la primera consulta tarda un poco.
- **Olvidar `sslmode=require`.** Neon exige conexión cifrada.
- **Dejar la cadena de conexión en el repo.** Es un secreto.
- **Pasar el cómputo gratuito sin darte cuenta.** El servicio se suspende hasta el siguiente periodo.

### Para profundizar
- [Neon](https://neon.com/)
- [Branching en Neon](https://neon.com/docs/introduction/branching)
- [Límites del plan gratuito](https://neon.com/docs/introduction/free-tier)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es Neon?
> PostgreSQL serverless: separa almacenamiento y cómputo, escala a cero cuando no hay uso y permite crear ramas (branches) de la base como si fueran ramas de Git.

> [!question]- ¿Para qué sirven las ramas de base de datos?
> Para tener una copia de la base por pull request o por entorno de pruebas, creada en segundos sin duplicar todo el almacenamiento.

> [!question]- ¿Qué cuidados tiene el plan gratuito?
> Límites de almacenamiento y horas de cómputo; al escalar a cero, la primera consulta tiene una pequeña latencia de arranque (verificar en la página oficial). Usa la cadena de conexión con `sslmode=require` y guárdala como secreto.

