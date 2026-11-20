---
proyecto: Conocimiento
tipo: certificacion
proveedor: Linux Foundation / CNCF
codigo: "KCNA · CKAD · CKA"
estado_cert: pendiente
fecha_examen:
fecha_obtenida:
vence:
credencial_id:
enlace_verificacion:
costo_usd: 250 / 445 / 445
vigencia: "2 años"
verificado_el: 2026-11-09
area: DevOps
tags: [certificacion, certificaciones]
---
# Kubernetes (KCNA, CKAD y CKA)

Parte de: [[18-Certificaciones/05 DevOps, contenedores y nube en general/_Índice DevOps, contenedores y nube en general|DevOps, contenedores y nube en general]] · Relacionado: [[Terraform Associate]]

> [!info] Estado de la certificación (`estado_cert`)
> **pendiente** → **preparando** → **lista** (cumples todos los criterios de abajo) → **agendada** (pon `fecha_examen`) → **obtenida** (llena fecha, `vence`, `credencial_id`, `enlace_verificacion` y adjunta el certificado) → **vencida** si no renuevas.

## Datos del examen

| Dato | Valor |
|---|---|
| Nombre | KCNA (Kubernetes and Cloud Native Associate) · CKAD (Application Developer) · CKA (Administrator) |
| Código | KCNA, CKAD, CKA |
| Formato | KCNA: opción múltiple, 90 min · CKAD y CKA: práctico en línea de comandos, 2 horas, Kubernetes v1.37 |
| Precio | KCNA 250 USD · CKAD 445 USD · CKA 445 USD (solo examen; hay paquetes y descuentos frecuentes) |
| Incluye | Un reintento; CKAD y CKA traen 2 sesiones del simulador killer.sh |
| Idiomas | Pendiente de confirmar (hay versiones -JP en japonés) |
| Requisitos | Ninguno |
| Dónde | En línea con supervisión; tienes 12 meses para presentar desde la compra |
| Vigencia | 2 años |

> Datos revisados el 2026-11-09. Confírmalos en la página oficial antes de pagar.

Fuentes: [1](https://training.linuxfoundation.org/certification/kubernetes-cloud-native-associate/) · [2](https://training.linuxfoundation.org/certification/certified-kubernetes-application-developer-ckad/) · [3](https://training.linuxfoundation.org/certification/certified-kubernetes-administrator-cka/)

## Dónde y cómo hacerla

1. Elige ruta: KCNA (conceptos) → CKAD (si desarrollas) o CKA (si administras).
2. Compra en training.linuxfoundation.org (espera promociones) y activa el simulador killer.sh.
3. Agenda dentro de los 12 meses y revisa los requisitos del equipo y del cuarto para el examen supervisado.

## ¿Cuándo estoy listo?

- [ ] Todas las notas de 08-Automatizacion-DevOps sobre contenedores en `visto` o más.
- [ ] Para CKAD/CKA: resuelvo con `kubectl` (sin interfaz) Deployments, Services, ConfigMaps/Secrets, probes, NetworkPolicies y PV/PVC en un clúster local (kind o minikube).
- [ ] CKA: hago respaldo/restauración de etcd y actualizo un clúster con kubeadm.
- [ ] Termino las dos sesiones de killer.sh con 85 % o más dentro del tiempo.
- [ ] Repasé el temario oficial punto por punto y cada punto tiene una nota mía.

Cuando todo esté marcado, cambia `estado_cert` a **lista** y pídeme una revisión: reviso tu avance en la bóveda y te digo qué reforzar.

## Mi avance en los temas

```base
filters:
  and:
    - or:
        - file.inFolder("08-Automatizacion-DevOps")
        - file.inFolder("07-Azure/03 Cómputo")
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
> 2. Guárdalos en la carpeta `Certificados` junto a esta nota como `Kubernetes (KCNA, CKAD y CKA) - certificado.pdf`.
> 3. Quita el `%%` de la línea de abajo para verlo aquí.
> 4. Llena `fecha_obtenida`, `vence`, `credencial_id` y `enlace_verificacion`.

%% ![[Kubernetes (KCNA, CKAD y CKA) - certificado.pdf]] %%

## Renovación

- [ ] Vencen a los 2 años: vuelve a presentar. Pon `vence`.
