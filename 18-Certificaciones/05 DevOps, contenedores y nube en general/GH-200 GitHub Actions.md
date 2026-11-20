---
proyecto: Conocimiento
tipo: certificacion
proveedor: Microsoft
codigo: "GH-200"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 
vigencia: "2 años"
verificado_el: 2026-11-09
area: DevOps
tags: [certificacion, certificaciones]
---
# GH-200 GitHub Actions

Parte de: [[18-Certificaciones/05 DevOps, contenedores y nube en general/_Índice DevOps, contenedores y nube en general|DevOps, contenedores y nube en general]] · Paso previo: [[GH-900 GitHub Foundations]] · Relacionado: [[AZ-400 DevOps Engineer Expert]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | GitHub Actions |
| Código | GH-200 |
| Duración | 100 minutos |
| Precio | Varía por país o región (más impuestos); el precio exacto aparece al agendar en Pearson VUE |
| Idiomas | Inglés, español, portugués (Brasil), coreano y japonés |
| Requisitos | Ninguno formal (experiencia intermedia con Actions) |
| Dónde | Pearson VUE: centro de examen o en línea (OnVUE) |
| Vigencia | 2 años |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://learn.microsoft.com/en-us/credentials/certifications/github-actions/)

## Dónde y cómo hacerla

1. Inicia sesión en Microsoft Learn con una cuenta personal (MSA), no la del trabajo: si dejas la empresa pierdes el historial.
2. En la página de la certificación elige **Programar examen** → Pearson VUE (centro de examen o en línea con OnVUE).
3. Revisa el precio que te muestra para México, paga con tarjeta o voucher y elige idioma (si no está en tu idioma puedes pedir 30 min extra).
4. Si repruebas puedes reintentar a las 24 h; los siguientes intentos tienen esperas mayores.

## ¿Cuándo estoy listo?

- [ ] Todas las notas de 08-Automatizacion-DevOps/01 Automatización y CICD en `visto` o más.
- [ ] Escribo un workflow con matriz, caché, artefactos, entornos con aprobación y OIDC hacia Azure.
- [ ] Publico una acción propia (JavaScript o compuesta) y la uso desde otro repositorio.
- [ ] 85 % o más en dos evaluaciones de práctica oficiales de Microsoft seguidas.
- [ ] Repasé el temario oficial (study guide) punto por punto y cada punto tiene una nota mía.

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("08-Automatizacion-DevOps/01 Automatización y CICD")
        - file.inFolder("02-Fundamentos/07 Git")
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `GH-200 GitHub Actions - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[GH-200 GitHub Actions - certificado.pdf]] %%

## Renovación

Vale 2 años. GitHub está pasando al proceso de renovación de Microsoft (sin repetir todo el examen); las que vencían antes de que exista se extendieron 6 meses.

- [ ] Pon `vence` (2 años después) y un recordatorio 6 meses antes.
