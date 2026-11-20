---
proyecto: Conocimiento
tipo: concepto
nivel: avanzado
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Redes de contenedores (Docker y Kubernetes)

Parte de: [[19-Redes/09 Avanzado/_Índice Avanzado|Avanzado]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Los contenedores tienen su propia pila de red aislada (un *network namespace* de Linux): interfaz, IP, tabla de rutas y puertos. **Docker** y **Kubernetes** crean redes virtuales para que los contenedores se comuniquen entre sí, con el host y con el exterior. Detrás hay conceptos que ya conoces del CCNA: bridges (switches virtuales), NAT, DNS, subredes y reglas de filtrado.

### Cómo funciona
**Drivers de red de Docker**:
- **bridge** (predeterminado): Docker crea un switch virtual en el host (la red `bridge`, con interfaz `docker0` y subred `172.17.0.0/16` por defecto). Cada contenedor recibe una IP privada y sale a internet con **NAT** (masquerade) del host.
- **Red bridge definida por el usuario** (`docker network create`): igual, pero con **resolución DNS por nombre** entre contenedores, mejor aislamiento (solo se ven los contenedores de esa red) y la posibilidad de conectar o desconectar contenedores en caliente. Por eso se recomienda crear redes propias en lugar de usar la predeterminada, donde los contenedores solo se encuentran por IP.
- **host**: el contenedor usa directamente la pila de red del host, sin aislamiento ni NAT. Máximo rendimiento, pero puertos compartidos con el host.
- **none**: sin red.
- **overlay**: une contenedores de varios hosts (Docker Swarm) con túneles VXLAN.
- **macvlan / ipvlan**: el contenedor aparece en la LAN física con su propia MAC o IP, como un equipo más.

**Publicación de puertos**: `-p 8080:80` significa "el puerto **8080 del host** se reenvía al puerto **80 del contenedor**" (formato `host:contenedor`). Por dentro es un NAT de destino (DNAT) que Docker configura en iptables o nftables del host. Puedes limitarlo a una IP: `-p 127.0.0.1:8080:80` solo escucha en el loopback.

**Kubernetes** asume un modelo plano: **cada Pod tiene su propia IP** y todos los Pods pueden hablar entre sí sin NAT; lo implementa un plugin **CNI** (Calico, Cilium, Azure CNI…).
- Las IP de los Pods cambian cuando se recrean, así que se usa un **Service**: una IP virtual estable y un nombre DNS (`api.produccion.svc.cluster.local`) que balancea hacia los Pods que coinciden con un selector de etiquetas. Tipos: **ClusterIP** (interno), **NodePort** (un puerto en cada nodo), **LoadBalancer** (un balanceador externo de la nube).
- **Ingress** o **Gateway API**: enrutamiento HTTP por host y ruta hacia los Services.
- **NetworkPolicy**: reglas de **firewall entre Pods** (por etiquetas, namespaces y puertos). Por defecto todo Pod puede hablar con todo Pod; una NetworkPolicy que selecciona un Pod lo aísla y solo permite lo declarado. Requiere un CNI que las soporte.

### Ejemplo
API de Itzamna.Dionisio y SQL Server en contenedores con una red propia:

```bash
docker network create --subnet 172.30.0.0/24 red-dionisio
docker run -d --name sql --network red-dionisio \
  -e ACCEPT_EULA=Y -e MSSQL_SA_PASSWORD="$SA_PASS" mcr.microsoft.com/mssql/server:2022-latest
docker run -d --name api --network red-dionisio -p 8080:8080 \
  -e ConnectionStrings__Default="Server=sql;Database=Dionisio;User Id=sa;Password=$SA_PASS;TrustServerCertificate=True" \
  itzamna/dionisio-api:dev
docker network inspect red-dionisio
docker exec api getent hosts sql      # resuelve "sql" a su IP en 172.30.0.0/24
```

La API encuentra la base por el nombre `sql` gracias al DNS de la red definida por el usuario, y solo la API publica un puerto al host; SQL no queda expuesto. Elige una subred que no choque con tu VPN o tu LAN.

En Kubernetes, permitir que solo la API llegue a SQL:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: sql-solo-desde-api
  namespace: produccion
spec:
  podSelector:
    matchLabels: { app: sql }
  policyTypes: [Ingress]
  ingress:
    - from:
        - podSelector:
            matchLabels: { app: api }
      ports:
        - protocol: TCP
          port: 1433
```

### Cuándo usarlo y cuándo no
Usa redes bridge propias para entornos de desarrollo con Docker Compose y NetworkPolicies en todo clúster de Kubernetes con más de una aplicación. La red `host` solo cuando el rendimiento o un protocolo especial lo exigen. En AKS, planea el direccionamiento de nodos y Pods con la VNet (ver [[Redes en la nube (VNet, subnets, NSG y peering)]]).

### Errores comunes
- **Usar la red bridge predeterminada** y esperar resolución por nombre.
- **Subred de Docker traslapada** con la VPN corporativa: deja de verse la red de la empresa.
- **Publicar puertos de bases de datos** al host o a `0.0.0.0` sin necesidad.
- **Creer que una NetworkPolicy funciona** con un CNI que no las implementa.

### Para profundizar
- [Documentación de Docker: redes](https://docs.docker.com/engine/network/)
- [Kubernetes: Services](https://kubernetes.io/docs/concepts/services-networking/service/)
- [Kubernetes: Network Policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué drivers de red básicos tiene Docker?
> **bridge** (predeterminado, red privada en el host), **host** (usa la red del host), **none** (sin red), **overlay** (entre hosts) y **macvlan** (MAC propia en la LAN).

> [!question]- ¿Por qué crear una red bridge propia en vez de usar la predeterminada?
> En una red definida por el usuario los contenedores se resuelven **por nombre** (DNS interno) y quedan aislados de otros. En la bridge predeterminada no hay resolución por nombre.

> [!question]- ¿Qué significa `-p 8080:80`?
> Publica el puerto 80 del contenedor en el 8080 del host.

> [!question]- En Kubernetes, ¿qué hace un Service y qué es una NetworkPolicy?
> Un **Service** da una IP y nombre estables a un grupo de pods (ClusterIP, NodePort, LoadBalancer). Una **NetworkPolicy** restringe qué pods pueden hablar con cuáles; requiere un CNI que la soporte (por defecto todo está permitido).

