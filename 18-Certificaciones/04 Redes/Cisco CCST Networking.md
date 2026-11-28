---
proyecto: Conocimiento
tipo: certificacion
proveedor: Cisco
codigo: "100-150"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 125
vigencia: "Sin vencimiento (Pendiente de confirmar en la página oficial)"
verificado_el: 2026-11-09
area: Redes
tags: [certificacion, redes, cisco]
contenido: completo
---
# Cisco CCST Networking (100-150)

Parte de: [[18-Certificaciones/04 Redes/_Índice Redes|Certificaciones de redes]] · Siguiente paso: [[CCNA]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | Cisco Certified Support Technician (CCST) Networking |
| Código | 100-150 |
| Nivel | Entrada; Cisco lo presenta como primer paso hacia CCNA |
| Duración | 50 minutos |
| Precio | 125 USD más impuestos |
| Idiomas | Incluye **español** |
| Requisitos previos | Ninguno |
| Dónde | Pearson VUE (centro o en línea) o Certiport (escuelas) |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial de Cisco antes de pagar.

## Dónde y cómo hacerla

1. Crea tu cuenta Cisco y estudia gratis en [[Cisco Networking Academy (cursos y Packet Tracer)]].
2. Agenda en Pearson VUE (centro de examen o en línea desde casa) o en un centro Certiport si tu escuela lo ofrece.
3. Llega con identificación oficial; en línea, revisa antes los requisitos de cámara y del cuarto.

## ¿Cuándo estoy listo?

- [ ] Todas las notas de [[19-Redes/01 Fundamentos/_Índice Fundamentos|Fundamentos]], [[_Índice Direccionamiento IP|Direccionamiento IP]] y [[_Índice Servicios de red|Servicios de red]] en `visto` o más.
- [ ] [[Lab 1 - Subredes IPv4 en Packet Tracer]] en `dominado`.
- [ ] Hago subnetting de una /24 sin calculadora en menos de 1 minuto.
- [ ] Saco **85 % o más en dos exámenes de práctica seguidos** (de fuentes oficiales o reconocidas; nunca "dumps").
- [ ] Puedo explicar con mis palabras cada tema del temario oficial del examen (descárgalo de Cisco y compáralo con estas notas).

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("19-Redes/01 Fundamentos")
        - file.inFolder("19-Redes/03 Direccionamiento IP")
        - file.inFolder("19-Redes/04 Servicios de red")
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
> 1. Descarga el certificado en PDF y la insignia digital (Cisco las entrega por Credly).
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota con el nombre `CCST Networking - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para que se vea aquí mismo.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion` en las propiedades.

%% ![[CCST Networking - certificado.pdf]] %%
