---
proyecto: Conocimiento
tipo: certificacion
proveedor: INE
codigo: "eJPT"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 
vigencia: "3 años"
verificado_el: 2026-11-09
area: Seguridad
tags: [certificacion, certificaciones]
---
# eJPT

Parte de: [[18-Certificaciones/06 Seguridad/_Índice Seguridad|Seguridad]] · Siguiente: [[OSCP]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | eLearnSecurity Junior Penetration Tester (eJPT) |
| Código | eJPT (la compra se identifica como v2: Pendiente de confirmar versión) |
| Duración | Pendiente de confirmar |
| Formato | Práctico, calificado automáticamente; dominios: pentest de host y red 35 %, metodologías 25 %, auditoría 25 %, web 15 % |
| Precio | Pendiente de confirmar (voucher o paquete con suscripción INE) |
| Idiomas | Pendiente de confirmar |
| Requisitos | Ninguno formal; voucher de examen |
| Dónde | En línea desde la plataforma de INE |
| Vigencia | 3 años; el voucher vence a los 180 días; incluye un reintento gratis dentro de 14 días |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://ine.com/security/certifications/ejpt-certification)

## Dónde y cómo hacerla

1. Toma la ruta de preparación de INE (Penetration Testing Student).
2. Compra el voucher y presenta el examen y su reintento antes de 180 días.

## ¿Cuándo estoy listo?

- [ ] Todas las notas de 12-Seguridad/06 Hacking ético en `visto` o más.
- [ ] Hago escaneo con nmap, enumeración de servicios, explotación con Metasploit y pivoting básico en máquinas fáciles de TryHackMe o Hack The Box.
- [ ] Terminé los laboratorios de la ruta de INE sin ver soluciones.
- [ ] Repasé el temario oficial punto por punto y cada punto tiene una nota mía.

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("12-Seguridad/06 Hacking ético")
        - file.inFolder("19-Redes/03 Direccionamiento IP")
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `eJPT - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[eJPT - certificado.pdf]] %%

## Renovación

- [ ] Vence a los 3 años. Pon `vence`.
