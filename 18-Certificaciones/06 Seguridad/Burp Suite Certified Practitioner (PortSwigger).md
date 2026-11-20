---
proyecto: Conocimiento
tipo: certificacion
proveedor: PortSwigger
codigo: "BSCP"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 
vigencia: "5 años"
verificado_el: 2026-11-09
area: Seguridad
tags: [certificacion, certificaciones]
---
# Burp Suite Certified Practitioner (PortSwigger)

Parte de: [[18-Certificaciones/06 Seguridad/_Índice Seguridad|Seguridad]] · Prepárate con: Web Security Academy de PortSwigger (gratis)

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | Burp Suite Certified Practitioner |
| Código | BSCP |
| Duración | 4 horas |
| Formato | Práctico: 2 aplicaciones, cada una en 3 etapas (entrar como usuario, llegar al panel de admin, leer `/home/carlos/secret`) |
| Precio | Pendiente de confirmar (se cobra en USD, GBP o EUR en la página de compra) |
| Idiomas | Inglés |
| Requisitos | Licencia activa de **Burp Suite Professional** (no sirve Community), crédito de examen y equipo que cumpla los requisitos |
| Dónde | En línea, con supervisión automatizada (identificación oficial, cámara y micrófono) |
| Vigencia | 5 años |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://portswigger.net/web-security/certification) · [2](https://portswigger.net/web-security/certification/how-it-works) · [3](https://portswigger.net/web-security/certification/frequently-asked-questions)

## Dónde y cómo hacerla

1. Termina los laboratorios Apprentice y Practitioner de Web Security Academy y el examen de práctica.
2. Consigue Burp Suite Professional activo para el día del examen.
3. Compra el crédito de examen (no caduca) y preséntalo cuando quieras; un reintento requiere comprar otro.

## ¿Cuándo estoy listo?

- [ ] Todas las notas de 12-Seguridad (Inyección SQL, XSS, Otras vulnerabilidades web) en `dominado`.
- [ ] Resolví todos los laboratorios Practitioner de Web Security Academy de los temas del examen.
- [ ] Aprobé el examen de práctica oficial en menos de 2 horas.
- [ ] Repasé el temario oficial punto por punto y cada punto tiene una nota mía.

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("12-Seguridad/02 Inyección SQL")
        - file.inFolder("12-Seguridad/03 XSS")
        - file.inFolder("12-Seguridad/04 Otras vulnerabilidades web")
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `Burp Suite Certified Practitioner (PortSwigger) - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[Burp Suite Certified Practitioner (PortSwigger) - certificado.pdf]] %%

## Renovación

- [ ] Vence a los 5 años (la fecha aparece en el certificado). Pon `vence`.
