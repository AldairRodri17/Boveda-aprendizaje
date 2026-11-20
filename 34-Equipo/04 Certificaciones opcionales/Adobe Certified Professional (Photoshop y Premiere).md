---
proyecto: Conocimiento
tipo: certificacion
proveedor: Adobe / Certiport
codigo: "ACP"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 
vigencia: "5 años (a partir del 21-oct-2026)"
verificado_el: 2026-11-09
area: Equipo
tags: [certificacion, equipo]
---
# Adobe Certified Professional (Photoshop y Premiere)

Parte de: [[34-Equipo/04 Certificaciones opcionales/_Índice Certificaciones opcionales|Certificaciones opcionales]] · Opcional: requiere software de pago (Photoshop / Premiere Pro); tu ruta gratuita es [[Certificación de DaVinci Resolve (Blackmagic)]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | Adobe Certified Professional: Visual Design Using Adobe Photoshop / Digital Video Using Adobe Premiere |
| Formato | Examen supervisado con preguntas de opción y sección de tareas dentro de la aplicación |
| Duración | 50 minutos, 30 preguntas |
| Calificación | Aprobar con 700 de 1000 |
| Experiencia sugerida | ~150 horas de instrucción y práctica con la aplicación |
| Dónde | Centro autorizado Certiport (CATC) o en línea con OnVUE (OnVUE solo en inglés) |
| Especialidad | Aprobar Premiere + Photoshop (o After Effects) otorga la especialidad Video Design |
| Precio | Varía por país (Pendiente de confirmar en Certiport) |
| Vigencia | 5 años (antes 3), desde el 21 de octubre de 2026 |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://certifiedprofessional.adobe.com/en/faq) · [2](https://certiport.pearsonvue.com/Blog/2026/September/Adobe-certifications-expiration-extension.aspx) · [3](https://certiport.pearsonvue.com/Certifications/Adobe/ACP/Certify.aspx)

## Dónde y cómo hacerla

1. Ojo: Photoshop y Premiere Pro son **de pago**; para practicar necesitarás una suscripción o la prueba gratuita de Adobe (revisa condiciones).
2. Revisa la guía del examen (objetivos) en la [página de ACP en Certiport](https://certiport.pearsonvue.com/Certifications/Adobe/ACP/Certify.aspx).
3. Crea tu cuenta en Certiport y busca un centro autorizado en México o agenda OnVUE (en inglés).
4. Presenta primero Premiere (video) y luego Photoshop si quieres la especialidad Video Design.

## ¿Cuándo estoy listo?

- [ ] Tengo acceso legal a Photoshop/Premiere Pro para practicar.
- [ ] Cubrí cada objetivo de la guía del examen con una nota o un proyecto propio.
- [ ] Completé al menos 2 proyectos de video de principio a fin en Premiere.
- [ ] Pasé un examen de práctica oficial o de un proveedor autorizado con margen.

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("33-Edicion-Video/02 Montaje")
        - file.inFolder("32-Edicion-Foto/04 Ajustes locales y retoque")
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `Adobe Certified Professional (Photoshop y Premiere) - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[Adobe Certified Professional (Photoshop y Premiere) - certificado.pdf]] %%

## Renovación

Las certificaciones ACP valen 5 años desde el 21 de octubre de 2026 (antes 3 años); se renuevan aprobando de nuevo el examen vigente.
