---
proyecto: Conocimiento
tipo: guia
estado: revisada
area: Reglas
tags: [reglas, mantenimiento]
contenido: completo
ultimo_commit_revisado: PENDIENTE
ultima_revision: 2026-11-28
---
# Reglas de la bóveda

Cómo se mantiene esta bóveda para que siempre esté limpia y se pueda estudiar con [[Temario]] como único índice. Aplican igual si editas desde Obsidian o si Claude edita desde GitHub.

## 1. Ninguna nota vacía

- Toda nota tiene contenido real debajo del título. Una nota que solo tiene frontmatter, o que tiene secciones `##` sin texto, cuenta como vacía.
- Las notas de estudio llevan el formato estándar: frontmatter con `progreso`, **Contenido**, **Laboratorio** (o Práctica) y **Autoevaluación** con callouts `> [!question]-`.
- Si una sección se llena después (por ejemplo "Mis errores frecuentes" en un laboratorio), se deja una línea en cursiva que diga cuándo y cómo se llena.
- Excepción: las plantillas de `Plantillas/` están vacías a propósito.

### Marca de nota completa

- Una nota que pasa la revisión lleva en su frontmatter `contenido: completo`.
- La revisión **se salta** las notas marcadas, salvo que hayan cambiado desde el último commit revisado.
- Si editas una nota marcada y le quitas contenido, quítale también la marca.

### Revisión incremental con Git

El último commit revisado se guarda en el frontmatter de esta nota (`ultimo_commit_revisado`). Cada revisión hace esto:

1. `git diff --name-only <ultimo_commit_revisado> HEAD -- '*.md'` para saber qué notas cambiaron.
2. Revisa las notas que cambiaron y las que no tienen `contenido: completo`. Las demás se saltan.
3. Marca las que pasan, reporta las que no, y actualiza `ultimo_commit_revisado` y `ultima_revision`.

## 2. Toda nota vive en su carpeta

- En la raíz solo van `Temario.md` y la configuración de Obsidian. Ninguna nota suelta.
- Las notas nuevas sin carpeta clara van a `00-Inbox/` y se mueven cuando se clasifican.
- **No des clic a enlaces de notas que todavía no existen.** Obsidian crea una nota vacía en la raíz. Para crear una nota pendiente, créala directamente en la carpeta de su sección.

## 3. Ningún enlace roto

Un `[[enlace]]` tiene que apuntar a una nota o archivo que exista. Hay tres casos que **no** cuentan como rotos:

| Caso | Ejemplo | Qué hacer |
|---|---|---|
| Nota planeada | Enlaces en el Temario o en un `_Índice` a temas que aún no tienen nota | Se lleva la cuenta en [[Pendientes de llenado]]; se resuelve al crear la nota |
| Nota de proyecto | `[[Itzamna.Dionisio]]`, `[[Ocelotl.Hermes]]` y sus módulos (Ingresos, Dispensar…) | Viven en el repositorio de cada proyecto; ver [[_Índice Proyectos]] |
| Adjunto pendiente | Certificados `.pdf` de `18-Certificaciones` | Se resuelve al subir el archivo |

Todo lo demás es un enlace roto y se corrige: nombre mal escrito, nota renombrada o movida, caracteres que el nombre de archivo no permite (por ejemplo `:`). Al renombrar una nota, actualizar todos sus enlaces en el mismo commit.

## 4. Repaso espaciado

- Cuando una nota pasa a `visto` o `dominado`, se agrega `proximo_repaso: AAAA-MM-DD` al frontmatter.
- Intervalos: `visto` → 7 días. Cada repaso que se contesta bien sin ayuda alarga el siguiente: 7 → 21 → 60 → 120 días. `dominado` → 60 días.
- Si un repaso sale mal, la nota vuelve a `viendo`, se le quita `proximo_repaso` y se dice qué repasar.
- Al empezar una sesión de estudio, primero se proponen las notas con `proximo_repaso` vencido.

## 5. Cambios desde GitHub

- Antes de editar: `git pull --rebase`.
- Un commit por cambio, con mensaje claro, y push directo a `main`. Nunca `push --force`.
- Si editas en Obsidian y en GitHub el mismo día, haz pull antes de abrir Obsidian para no pisar cambios.

## Última revisión

**28 nov 2026** · revisión completa de 2,500 notas.

- **Notas marcadas `contenido: completo`:** 2,491 (todas menos las 2 plantillas).
- **Notas vacías encontradas:** 7, todas archivos de 0 bytes en la raíz creados por clic en enlaces pendientes; 6 tenían su nota real en su carpeta y se borraron junto con `Sin título.canvas` y el índice obsoleto `_Índice C# y .NET`.
- **Secciones vacías:** 1 ("Mis errores frecuentes" del Lab 4 de inglés), se le puso la indicación de cómo llenarla.
- **Enlaces rotos corregidos:** 1 destino (`Lab 7 - Posprocesado: soportes…`, el nombre del archivo no lleva `:`), en 5 notas. Los enlaces a `Temario v2` (182 notas) ahora apuntan a [[Temario]].
- **No son rotos (ver tabla de la regla 3):** 828 notas planeadas en índices, 66 entre notas de proyecto (y sus módulos) y temas citados que aún no tienen nota, y 46 adjuntos `.pdf` pendientes.
- **Repaso espaciado:** ninguna nota está todavía en `visto` o `dominado`, así que ninguna lleva `proximo_repaso` aún.
