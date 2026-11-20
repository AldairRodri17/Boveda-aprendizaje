---
proyecto: Conocimiento
tipo: certificacion
proveedor: Cisco
codigo: "200-301"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 300
vigencia: "3 años"
verificado_el: 2026-11-09
area: Redes
tags: [certificacion, redes, cisco]
---
# CCNA (200-301)

Parte de: [[18-Certificaciones/04 Redes/_Índice Redes|Certificaciones de redes]] · Paso previo opcional: [[Cisco CCST Networking]] · Siguiente: [[CCNP Enterprise]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | Implementing and Administering Cisco Solutions (CCNA) |
| Código | 200-301 |
| Duración | 120 minutos |
| Precio | 300 USD más impuestos |
| Idiomas | Inglés y japonés (no hay en español: ver [[Inglés técnico - plan de estudio]]) |
| Requisitos previos | Ninguno |
| Dónde | Pearson VUE: centro de examen o en línea (OnVUE) |
| Vigencia | 3 años; se renueva con examen o créditos de educación continua |
| Versión | Hoy v1.1. Hay una **v2.0** anunciada a partir del 3 de febrero de 2027 (Pendiente de confirmar en la página oficial de Cisco); si presentas después de esa fecha, estudia con el temario nuevo. |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial de Cisco antes de pagar.

## Dónde y cómo hacerla

1. Crea tu cuenta Cisco y vincúlala en Pearson VUE (pearsonvue.com/cisco).
2. Busca el examen 200-301 y elige centro de examen u OnVUE en línea.
3. Paga con tarjeta o voucher; compra vouchers solo a Cisco o Pearson VUE.
4. Un reintento se paga completo y hay espera entre intentos; reprogramar con poca anticipación tiene costo. Revisa las políticas al agendar.

## ¿Cuándo estoy listo?

- [ ] Todas las notas de [[19-Redes/_Índice Redes|19-Redes]] (excepto las marcadas como avanzadas fuera de temario) en `visto` o más.
- [ ] Al menos el 80 % en `dominado`, con todos los laboratorios de Switching, Direccionamiento IP, Routing y Seguridad de redes hechos.
- [ ] Hago subnetting y VLSM sin calculadora en menos de 1 minuto por problema.
- [ ] Configuro desde cero, sin apuntes, una red con VLANs, trunk, routing entre VLANs, OSPF, DHCP, NAT y ACLs en Packet Tracer.
- [ ] Saco **85 % o más en dos exámenes de práctica seguidos** (fuentes oficiales o reconocidas; nunca "dumps").
- [ ] Repasé el temario oficial del examen punto por punto y cada punto tiene una nota mía.

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("19-Redes")
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota con el nombre `CCNA - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para que se vea aquí mismo.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion` en las propiedades.

%% ![[CCNA - certificado.pdf]] %%

## Renovación

- [ ] Cuando la obtengas, pon `vence` (3 años después) y crea un recordatorio 6 meses antes.
