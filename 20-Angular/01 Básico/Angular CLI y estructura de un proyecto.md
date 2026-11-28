---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Angular
tags: [angular]
contenido: completo
---
# Angular CLI y estructura de un proyecto

Parte de: [[20-Angular/01 Básico/_Índice Básico|Básico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El **Angular CLI** (`ng`) es la herramienta oficial de línea de comandos para crear, generar, servir, probar, compilar y actualizar proyectos Angular. Equivale a lo que en React haces con Vite más varios scripts, pero con convenciones fijas. Por dentro usa el builder `@angular/build` (esbuild y el servidor de desarrollo de Vite); el soporte de Webpack quedó obsoleto en Angular 22.

### Cómo funciona
Instalas el CLI una vez y creas el proyecto:

```bash
# Instalar el CLI (requiere Node.js LTS reciente)
npm install -g @angular/cli
# Crear el proyecto; pregunta estilos, SSR y más
ng new itzamna-admin
cd itzamna-admin
ng serve -o          # servidor de desarrollo en http://localhost:4200
```

Comandos del día a día:

| Comando | Qué hace |
|---|---|
| `ng serve` | Compila en memoria y recarga al guardar |
| `ng generate` (`ng g`) | Genera componentes, servicios, pipes, guards, etc. |
| `ng test` | Corre pruebas unitarias (Vitest por defecto desde v21) |
| `ng build` | Compila para producción en `dist/` |
| `ng add <paquete>` | Instala y configura una librería (Material, SSR, localize) |
| `ng update` | Actualiza versiones y aplica migraciones automáticas |

#### Estructura que genera `ng new`
```text
itzamna-admin/
├─ angular.json          configuración del workspace y builders
├─ package.json
├─ tsconfig.json         (+ tsconfig.app.json y tsconfig.spec.json)
├─ public/               archivos estáticos (favicon, imágenes)
└─ src/
   ├─ index.html         página única con <app-root>
   ├─ main.ts            punto de entrada
   ├─ styles.css         estilos globales
   └─ app/
      ├─ app.ts          componente raíz (clase App)
      ├─ app.html / app.css / app.spec.ts
      ├─ app.config.ts   providers de la aplicación
      └─ app.routes.ts   rutas
```

Desde Angular 20 la guía de estilo quitó los sufijos: el archivo es `app.ts` y la clase `App`, no `app.component.ts`/`AppComponent`.

- **`src/main.ts`** arranca la aplicación: llama a `bootstrapApplication(App, appConfig)`.
- **`src/app/app.config.ts`** exporta un `ApplicationConfig` con la lista de **providers** globales: el router con sus rutas, el manejador de errores del navegador, interceptores HTTP, etc. Es el equivalente a configurar los `Provider` de React en la raíz o al `Program.cs` de ASP.NET Core.
- **`angular.json`** define los proyectos del workspace y, por cada uno, los *targets* (`build`, `serve`, `test`, `extract-i18n`) con sus opciones: archivos de estilos, presupuestos de tamaño, configuraciones `production`/`development`, proxy, idiomas.

### Ejemplo
Generar un componente de lista dentro de la feature de productos:

```bash
ng g c productos/lista
# CREATE src/app/productos/lista/lista.ts
# CREATE src/app/productos/lista/lista.html
# CREATE src/app/productos/lista/lista.css
# CREATE src/app/productos/lista/lista.spec.ts
```

Crea la clase `Lista` con selector `app-lista`, standalone (no hace falta declararla en ningún módulo). Otros generadores útiles: `ng g s productos/productos-api` (servicio, en v22 con `@Service()`), `ng g p shared/iniciales` (pipe), `ng g guard auth/sesion`, `ng g interceptor core/auth`. Agrega `--dry-run` para ver qué crearía sin escribir nada.

Así se ve un `app.config.ts` típico:

```ts
import { ApplicationConfig, provideBrowserGlobalErrorListeners } from '@angular/core';
import { provideRouter, withComponentInputBinding } from '@angular/router';
import { routes } from './app.routes';

export const appConfig: ApplicationConfig = {
  providers: [
    provideBrowserGlobalErrorListeners(),
    provideRouter(routes, withComponentInputBinding()),
  ],
};
```

### Cuándo usarlo y cuándo no
- Usa siempre el CLI para generar y actualizar; respeta nombres y carpetas y te ahorra errores.
- Para varios proyectos en un repo, el workspace de Angular admite múltiples apps y librerías (`ng g library`); para monorepos grandes, Nx es la alternativa común.
- No edites a mano `angular.json` sin saber qué builder lees; usa `ng add` y `ng config` cuando existan.

### Errores comunes
- **Actualizar con `npm install` en lugar de `ng update`.** Te saltas las migraciones automáticas y el código queda a medias. Actualiza de una versión mayor a la siguiente, no saltando varias.
- **Versión de Node incompatible.** Cada versión de Angular soporta rangos concretos de Node y TypeScript; Angular 22 dejó de soportar Node 20. Revisa la tabla de compatibilidad.
- **Poner archivos estáticos en `src/assets` por costumbre.** Los proyectos nuevos usan la carpeta `public/`.
- **Generar en la carpeta equivocada.** La ruta de `ng g` es relativa a `src/app`.

### Para profundizar
- [Angular CLI: visión general](https://angular.dev/tools/cli)
- [Referencia de comandos del CLI](https://angular.dev/cli)
- [Configuración del workspace (angular.json)](https://angular.dev/reference/configs/workspace-config)
- [Guía de actualización de versiones](https://angular.dev/update-guide)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué comandos del CLI usas a diario?
> `ng new` (crear proyecto) · `ng serve` (servidor de desarrollo en `http://localhost:4200`) · `ng generate` / `ng g` (componentes, servicios, guards, pipes…) · `ng build` (compila a `dist/`) · `ng test` · `ng add` (agrega bibliotecas) · `ng update`.

> [!question]- ¿Qué hacen `src/main.ts` y `src/app/app.config.ts`?
> `main.ts` arranca la app con `bootstrapApplication(App, appConfig)`. `app.config.ts` registra los *providers* globales: `provideRouter(routes)`, `provideHttpClient()`, etc.

> [!question]- ¿Para qué sirve `angular.json`?
> Es la configuración del workspace: proyectos, opciones de build y serve, estilos globales, assets, presupuestos de tamaño y configuraciones (development/production).

> [!question]- ¿Qué genera `ng g c productos/lista`?
> Una carpeta `src/app/productos/lista/` con el archivo TypeScript del componente, su plantilla HTML, sus estilos y su archivo de pruebas. (Los nombres exactos de archivo y clase dependen de la versión del CLI; puedes usar `--dry-run` para ver qué crearía).

## Práctica

- [[Lab 1 - Componentes standalone y data binding en Angular]]

