[Volver al índice general](../README.md)

# UD2 – Diseño técnico de la infraestructura empresarial segura y automatizada

## Índice de apartados

- [ ] **[1. Recopilación de información técnica](#1-recopilación-de-información-técnica)**
- [ ] **[2. Diseño lógico y físico de la infraestructura](#2-diseño-lógico-y-físico-de-la-infraestructura)**
- [ ] **[3. Definición de objetivos y fases del proyecto](#3-definición-de-objetivos-y-fases-del-proyecto)**
- [ ] **[4. Recursos y presupuesto](#4-recursos-y-presupuesto)**
- [ ] **[5. Documentación técnica](#5-documentación-técnica)**

## Apartados

### [1. Recopilación de información técnica](#índice-de-apartados)
![Debian](https://img.shields.io/badge/Debian-A81D33?style=for-the-badge&logo=debian&logoColor=white)
![Windows Server](https://img.shields.io/badge/Windows_Server-0078D6?style=for-the-badge&logo=windows&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)

La infraestructura se basa en una arquitectura de red segmentada en tres zonas diferenciadas para garantizar la seguridad, el aislamiento del tráfico y el rendimiento de los servicios.

#### Sistemas Operativos

- **Debian 13 (Trixie)** se utiliza como sistema operativo del servidor principal por su estabilidad, amplio soporte de la comunidad y bajo consumo de recursos, características ideales para un servidor que actúa como gateway y proxy. Al ser una distribución LTS ampliamente usada en entornos empresariales, garantiza compatibilidad con todas las herramientas del proyecto.

- **Windows Server 2022** se emplea para el rol de Active Directory y DNS. Se elige esta versión por ser la más reciente con soporte a largo plazo (LTSC), ofrece mejoras de seguridad respecto a versiones anteriores y es la versión estándar en entornos empresariales actuales. Se utilizará bajo licencia de evaluación de 180 días.

#### Base de Datos

- **AWS RDS (MySQL/PostgreSQL)** como servicio gestionado de base de datos en la nube. Se elige RDS frente a una base de datos local por delegar la gestión del mantenimiento, backups y alta disponibilidad en AWS, permitiendo centrarse en la arquitectura de la infraestructura. Se utilizará dentro de la capa gratuita (Free Tier).

#### Virtualización y Contenerización

- **VirtualBox** como hipervisor de tipo 2 para el despliegue de las máquinas virtuales en un entorno local. Se elige por ser gratuito, multiplataforma y suficiente para un entorno de laboratorio.

- **Docker** junto con **Docker Compose** para la contenerización del stack de monitorización. El uso de contenedores permite aislar los servicios de monitorización, facilitar su despliegue reproducible y simplificar futuras actualizaciones sin afectar al sistema anfitrión.

#### Stack de Monitorización

Se ha optado por el stack **TICK** como alternativa al stack Prometheus + Grafana + Node Exporter utilizado en clase:

- **Telegraf** actúa como agente de recopilación de métricas instalado directamente en cada servidor a monitorizar. Utiliza un sistema de plugins de entrada y salida, lo que permite recoger métricas de CPU, memoria, disco y red tanto en Linux como en Windows sin necesidad de exporters adicionales.
- **InfluxDB 2.7** es la base de datos de series temporales donde Telegraf almacena las métricas recogidas. Está optimizada para escrituras y consultas sobre datos con marca temporal, lo que la hace especialmente adecuada para métricas de monitorización.
- **Chronograf** es la interfaz web de visualización que se conecta a InfluxDB para mostrar dashboards y gestionar alertas.

```
monitoring/
├── docker-compose.yml
├── influxdb/
│   └── config.env
└── telegraf/
    ├── telegraf-windows.conf
    └── telegraf-debian.conf
```

A diferencia de Prometheus, que usa un modelo **pull** (el servidor va a buscar las métricas a los exporters), el stack TICK usa un modelo **push** (los agentes Telegraf envían las métricas directamente a InfluxDB), lo que simplifica la configuración de red al no requerir que el servidor de monitorización tenga acceso directo a cada host.

#### Seguridad y Red

- **OPNsense** como firewall perimetral para gestionar el tráfico entre las tres subredes y hacia Internet. Se elige frente a otras alternativas como pfSense por su interfaz más moderna, actualizaciones más frecuentes y mejor soporte de plugins.

- **WireGuard** como solución VPN para el acceso remoto seguro a la infraestructura. Se elige por su simplicidad de configuración, alto rendimiento y por estar integrado en el kernel de Linux desde la versión 5.6.

---

### [2. Diseño lógico y físico de la infraestructura](#índice-de-apartados)

<p align="center"><img src="/UD2/img/proyectoOscuro.png" alt="mapa" width="850" height="400"></p>

El diseño sigue un **modelo de defensa** en profundidad:

- **RED 0 (DMZ - 172.15.0.0)**: Zona desmilitarizada que actúa como frontera. Aquí se sitúa el servidor Linux que gestiona las peticiones externas antes de conectar con el backend.
- **RED 1 (Active Directory - 172.10.1.0)**: Red interna dedicada a la gestión de identidades y políticas de grupo mediante Windows Server.
- **RED 2 (Monitoreo - 192.168.50.0)**: Red aislada para el despliegue de contenedores (Docker) encargados del análisis de logs y métricas del sistema.
- **Conectividad Cloud**: El servidor Linux establece una conexión segura con AWS BBDD para la persistencia de datos.

---

### [3. Definición de objetivos y fases del proyecto](#índice-de-apartados)

#### Fase 1 – Networking
Configuración de la infraestructura de red base sobre la que se sustenta todo el proyecto:

- Instalación y configuración de **OPNsense** como firewall perimetral.
- Creación de las tres interfaces de red: RED 0 (DMZ), RED 1 (AD) y RED 2 (Monitoreo).
- Definición de reglas de enrutamiento y firewall entre subredes.
- Configuración de **WireGuard** para acceso VPN remoto a la infraestructura.
- Verificación de conectividad entre las tres redes.

#### Fase 2 – Servicios Core
Despliegue y configuración de los servicios principales de la infraestructura:

- Instalación de **Debian 13** en Srv-Linux-Main y configuración como gateway y proxy.
- Instalación de **Windows Server 2022** en Srv-Win-AD.
- Promoción del servidor Windows a controlador de dominio (**Active Directory**).
- Configuración del servicio **DNS** integrado en AD.
- Creación de unidades organizativas, usuarios y políticas de grupo (GPO).
- Unión del servidor Linux al dominio.

#### Fase 3 – Contenerización y Monitorización
Implementación del stack de monitorización mediante Docker:

- Instalación de **Docker** y **Docker Compose** en Srv-Docker.
- Despliegue del stack TICK mediante `docker-compose.yml` (**InfluxDB** + **Chronograf**).
- Instalación y configuración del agente **Telegraf** en Srv-Linux-Main y Srv-Win-AD.
- Verificación del envío de métricas desde ambos servidores a InfluxDB.
- Creación de dashboards en Chronograf para visualización de métricas.

#### Fase 4 – Integración Cloud
Configuración de la conectividad segura con AWS:

- Creación de la instancia **AWS RDS** dentro del Free Tier.
- Configuración de **Security Groups** para restringir el acceso a la BBDD únicamente desde Srv-Linux-Main.
- Configuración de la conexión entre Srv-Linux-Main y el endpoint de AWS RDS.
- Verificación de conectividad y pruebas de acceso a la base de datos.

---

### [4. Recursos y presupuesto](#índice-de-apartados)
![OPNsense](https://img.shields.io/badge/OPNsense-D94F00?style=for-the-badge&logo=opnsense&logoColor=white)
![WireGuard](https://img.shields.io/badge/WireGuard-88171A?style=for-the-badge&logo=wireguard&logoColor=white)
![Debian](https://img.shields.io/badge/Debian-A81D33?style=for-the-badge&logo=debian&logoColor=white)
![Windows Server](https://img.shields.io/badge/Windows_Server-0078D6?style=for-the-badge&logo=windows&logoColor=white)
![Active Directory](https://img.shields.io/badge/Active_Directory-0078D6?style=for-the-badge&logo=microsoft&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![InfluxDB](https://img.shields.io/badge/InfluxDB-22ADF6?style=for-the-badge&logo=influxdb&logoColor=white)
![Chronograf](https://img.shields.io/badge/Chronograf-22ADF6?style=for-the-badge&logo=influxdb&logoColor=white)
![Telegraf](https://img.shields.io/badge/Telegraf-22ADF6?style=for-the-badge&logo=influxdb&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazonwebservices&logoColor=white)
![Open Source](https://img.shields.io/badge/Open_Source-3DA639?style=for-the-badge&logo=opensourceinitiative&logoColor=white)

#### Hardware

Toda la infraestructura se desplegará mediante máquinas virtuales sobre un equipo anfitrión local utilizando **VirtualBox** como hipervisor de tipo 2. Se asignarán los siguientes recursos por máquina virtual:

| Máquina Virtual | CPU (vCores) | RAM | Almacenamiento |
|---|---|---|---|
| Srv-Linux-Main (Debian) | 2 | 2 GB | 20 GB |
| Srv-Win-AD (Windows Server) | 2 | 4 GB | 40 GB |
| Srv-Docker (Linux + Docker) | 2 | 4 GB | 30 GB |

#### Software

| Herramienta | Función | Coste |
|---|---|---|
| VirtualBox | Hipervisor para las máquinas virtuales | Gratuito |
| OPNsense | Firewall y enrutamiento entre redes | Gratuito |
| WireGuard | VPN para acceso seguro | Gratuito |
| Debian 12 | Sistema operativo servidor Linux | Gratuito |
| Windows Server 2022 | Active Directory y DNS | Licencia de evaluación (180 días) |
| Docker + Docker Compose | Contenerización del stack de monitorización | Gratuito |
| InfluxDB 2.7 | Base de datos de series temporales | Gratuito (Open Source) |
| Chronograf | Visualización de métricas | Gratuito (Open Source) |
| Telegraf | Agente de recopilación de métricas | Gratuito (Open Source) |

#### Cloud

| Servicio AWS | Descripción | Coste estimado |
|---|---|---|
| AWS RDS (Free Tier) | Base de datos gestionada SQL | Gratuito (750h/mes primer año) |
| AWS Security Groups | Control de acceso a la instancia RDS | Incluido |

#### Coste total estimado

| Concepto | Coste |
|---|---|
| Hardware (equipo anfitrión existente) | 0 € |
| Software | 0 € |
| Cloud (AWS Free Tier) | 0 € |
| **Total** | **0 €** |

El coste del proyecto es mínimo al basarse íntegramente en software de código abierto y en la capa gratuita de AWS. El único coste potencial sería superar los límites del Free Tier de AWS o la necesidad de adquirir una licencia de Windows Server más allá del periodo de evaluación.

---

### [5. Documentación técnica](#índice-de-apartados)

- Tabla de Direccionamiento

| Interfaz | Red | Segmento IP | Máscara | Puerta de Enlace (GW) | Descripción |
|---|---|---|---|---|---|
| RED 0 (DMZ) | `172.15.0.0/24` | `172.15.0.0` | `255.255.255.0` | `172.15.0.1` | Zona perimetral y Servidor Linux |
| RED 1 (AD) | `172.10.1.0/24` | `172.10.1.0` | `255.255.255.0` | `172.10.1.1` | Gestión de Active Directory (Windows) |
| RED 2 (Monitor) | `192.168.50.0/24` | `192.168.50.0` | `255.255.255.0` | `192.168.50.1` | Servicios de Monitoreo y Docker |
| WAN | DHCP / Static | N/A | N/A | N/A | Salida a Internet y Conexión AWS |

- Tabla de Hosts

| **Dispositivo** | **Dirección IP** | **Sistema Operativo** | **Servicio Principal** |
|---|---|---|---|
| **Srv-Linux-Main** | `172.15.0.10` | Debian 13 | Gateway / Proxy / AWS Bridge / Telegraf |
| **Srv-Win-AD** | `172.10.1.10` | Windows Server | Active Directory / DNS / Usuarios / Telegraf |
| **Srv-Docker** | `192.168.50.10` | Linux (Docker) | InfluxDB / Chronograf |
| **AWS BBDD** | `Endpoint URL` | Managed SQL | Base de Datos Externa |

---

## Enlaces a recursos de la unidad

- [Documentos de la unidad](./documentos/)
- [Diagramas e imágenes](./img/)

## Bibliografía / Webgrafía

- Debian. *Debian 13 "Trixie" Release Notes*. \
  🔗 [Debian 13](https://www.debian.org/releases/trixie/release-notes.es.pdf)
- Microsoft. *Windows Server 2022 – Ciclo de vida del producto*. Microsoft Learn, 2024. \
  🔗 [Windows Server](https://learn.microsoft.com/es-es/lifecycle/products/windows-server-2022)
- Microsoft. *Licencias de evaluación de Windows Server 2022*. Microsoft Evaluation Center. \
  🔗 [Licencias de Evaluación](https://www.microsoft.com/es-es/evalcenter/evaluate-windows-server-2022)
- Amazon Web Services. *AWS RDS – Free Tier*. AWS Documentation, 2024. \
  🔗 [AWS RDS](https://aws.amazon.com/es/rds/free)
- Docker Inc. *Docker Overview*. Docker Documentation, 2024. \
  🔗 [Docker](https://docs.docker.com/get-started/overview)
- InfluxData. *InfluxDB 2.7 Documentation*. InfluxData, 2024. \
  🔗 [InfluxDB](https://docs.influxdata.com/influxdb/v2.7)
- InfluxData. *Telegraf Documentation – Agent configuration*. InfluxData, 2024. \
  🔗 [Telegraf](https://docs.influxdata.com/telegraf/v1)
- InfluxData. *Chronograf Documentation*. InfluxData, 2024. \
  🔗 [Chronograf](https://docs.influxdata.com/chronograf/v1)
- OPNsense. *OPNsense vs pfSense – Feature Comparison*. OPNsense Documentation. \
  🔗 [OPNsense](https://docs.opnsense.org)
- WireGuard. *WireGuard – Fast, Modern, Secure VPN Tunnel*. WireGuard Project. \
  🔗 [WireGuard](https://www.wireguard.com)
