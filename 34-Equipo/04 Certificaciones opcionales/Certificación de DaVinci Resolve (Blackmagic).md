---
proyecto: Conocimiento
tipo: certificacion
proveedor: Blackmagic Design
codigo: "DaVinci Resolve Certified User"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 
vigencia: "No publicada (Pendiente de confirmar)"
verificado_el: 2026-11-09
area: Equipo
tags: [certificacion, equipo]
contenido: completo
---
# Certificación de DaVinci Resolve (Blackmagic)

Parte de: [[34-Equipo/04 Certificaciones opcionales/_Índice Certificaciones opcionales|Certificaciones opcionales]] · Relacionado: [[DaVinci Resolve (versión gratuita)]] · [[Edición de material aéreo]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | Certificación de usuario de DaVinci Resolve (por libro: Beginner's, Editor's, Colorist, Fairlight, Visual Effects) |
| Formato | Examen en línea al final de cada libro oficial de entrenamiento |
| Preguntas | 50 al azar de un banco de 120, 60 min, aprobar con 85 % (datos de la versión 18 publicados por Ripple Training; Pendiente de confirmar para la versión 20/21) |
| Reintentos | Dos intentos adicionales (versión 18; Pendiente de confirmar) |
| Precio | No se publica en la página de entrenamiento (Pendiente de confirmar) |
| Software | DaVinci Resolve gratuito alcanza para estudiar los libros de iniciación y edición |
| Vigencia | No publicada (Pendiente de confirmar) |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://www.blackmagicdesign.com/products/davinciresolve/training) · [2](https://support.rippletraining.com/article/77-dvr-18-test-faqs)

## Dónde y cómo hacerla

1. Entra a la página de [entrenamiento de DaVinci Resolve](https://www.blackmagicdesign.com/products/davinciresolve/training) y descarga el libro **The Beginner's Guide to DaVinci Resolve** de la versión más reciente con sus archivos de práctica.
2. Haz todas las lecciones en DaVinci Resolve gratuito, con los archivos del libro.
3. Al terminar, usa el enlace **Complete Online Exam** del libro.
4. Descarga tu certificado y guárdalo en `Certificados` junto a esta nota.
5. Siguiente paso opcional: el libro **Editor's Guide** o **Colorist Guide** y su examen.

## ¿Cuándo estoy listo?

- [ ] Terminé todas las lecciones del Beginner's Guide con sus archivos.
- [ ] Edito un video de 2–3 min de principio a fin (importar, cortar, color básico, audio, exportar) sin tutorial.
- [ ] Las notas de [[33-Edicion-Video/_Índice Edición de video|33-Edicion-Video]] sección 01 Fundamentos están en `visto` o más.
- [ ] Hice el [[Lab 5 - Edición de material aéreo en DaVinci Resolve]] de 35-Drones.

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("33-Edicion-Video/01 Fundamentos")
        - file.inFolder("33-Edicion-Video/04 Color")
    - tipo != "indice"
views:
  - type: table
    name: Lo que me falta
    filters:
      and:
        - progreso != "dominado"
    order:
      - file.name
      - progreso
      - file.folder
  - type: table
    name: Dominado
    filters:
      and:
        - progreso == "dominado"
    order:
      - file.name
      - file.folder
```

## Exámenes de práctica

| Fecha | Fuente | Resultado | Temas flojos |
|---|---|---|---|
|  |  |  |  |

## Mi certificado

> [!tip] Cuando la obtengas
> 1. Descarga el certificado (PDF) y la insignia digital.
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `Certificación de DaVinci Resolve (Blackmagic) - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[Certificación de DaVinci Resolve (Blackmagic) - certificado.pdf]] %%

## Renovación

Blackmagic no publica vencimiento; el certificado corresponde a la versión del libro. Cuando salga una versión mayor nueva, puedes volver a certificarte con el libro actualizado (Pendiente de confirmar).
