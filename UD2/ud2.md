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

La infraestructura se basa en una arquitectura de red segmentada para garantizar la **seguridad** y el **rendimiento**.

- **Sistemas Operativos**: Linux (Servidor principal) y Windows (Active Directory).
- **Base de Datos**: AWS RDS (Instancia gestionada de SQL).
- **Virtualización/Contenerización**: Docker para servicios de monitoreo.
- **Segmentación IP**: Uso de tres subredes distintas para aislar tráfico de gestión, servicios y datos.

---

### [2. Diseño lógico y físico de la infraestructura](#índice-de-apartados)

<p align="center"><img src="/UD2/img/proyectoOscuro.png" alt="mapa" width="850" height="400"></p>

El diseño sigue un **modelo de defensa** en profundidad:

- **RED 0 (DMZ - 172.15.0.0)**: Zona desmilitarizada que actúa como frontera. Aquí se sitúa el servidor Linux que gestiona las peticiones externas antes de conectar con el backend.
- **RED 1 (Active Directory - 172.10.1.0)**: Red interna dedicada a la gestión de identidades y políticas de grupo mediante Windows Server.
- **RED 2 (Monitoreo - 192.168.50.0)**: Red aislada para el despliegue de contenedores (Docker) encargados del análisis de logs y métricas del sistema.
- **Conectividad Cloud**: El servidor Linux establece una conexión segura con AWS BBDD para la persistencia de datos.

graph TD
    Internet["🌐 Internet / AWS"] --> DMZ
    subgraph DMZ ["RED 0 – DMZ 172.15.0.0/24"]
        Linux["Srv-Linux-Main\n172.15.0.10\nGateway / Proxy"]
    end
    Linux --> AD
    Linux --> Monitor
    Linux --> AWS
    subgraph AD ["RED 1 – Active Directory 172.10.1.0/24"]
        WinAD["Srv-Win-AD\n172.10.1.10\nAD / DNS"]
    end
    subgraph Monitor ["RED 2 – Monitoreo 192.168.50.0/24"]
        Docker["Srv-Docker\n192.168.50.10\nInfluxDB / Chronograf"]
    end
    AWS["AWS RDS\nEndpoint URL\nManaged SQL"]

---

### [3. Definición de objetivos y fases del proyecto](#índice-de-apartados)

1. **Fase 1 Networking**: Configuración de interfaces de red y reglas de enrutamiento entre las tres subredes.
2. **Fase 2 Servicios Core**: Despliegue del controlador de dominio (AD) y configuración del servidor Linux.
3. **Fase 3 Contenerización**: Implementación de la pila de monitoreo en Docker.
4. **Fase 4 Integración Cloud**: Configuración de conectividad y seguridad (Security Groups) con AWS.

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

- **Hardware**: Ordenador con máquinas virtuales (aún por ver dónde las creo).
- **Software**: OPNsense (firewall), WireGuard (VPN), Debian (servidor), Windows Server con AD (permisos y usuarios), Docker (monitorización con InfluxDB y Chronograf) y Telegraf (parecido a un exporter).
- **Cloud**: Uso de AWS para un servicio posterior.
- **Costo Estimado**: Mínimo (uso de software Open Source y capas gratuitas de Cloud).

---

### [5. Documentación técnica](#índice-de-apartados)

- Tabla de Direccionamiento

| **Interfaz Red** | **Segmento IP** | **Máscara** | **Puerta de Enlace (GW)** | **Descripción** |
|---|---|---|---|---|
| **RED 0 (DMZ)** | `172.15.0.0/24` | `255.255.255.0` | `172.15.0.1` | Zona perimetral y Servidor Linux |
| **RED 1 (AD)** | `172.10.1.0/24` | `255.255.255.0` | `172.10.1.1` | Gestión de Active Directory (Windows) |
| **RED 2 (Monitor)** | `192.168.50.0/24` | `255.255.255.0` | `192.168.50.1` | Servicios de Monitoreo y Docker |
| **WAN, DHCP** | Static | N/A | ISP Gateway | Salida a Internet y Conexión AWS |

- Tabla de Hosts

| **Dispositivo** | **Dirección IP** | **Sistema Operativo** | **Servicio Principal** |
|---|---|---|---|
| **Srv-Linux-Main** | `172.15.0.10` | Ubuntu / Debian | Gateway / Proxy / AWS Bridge |
| **Srv-Win-AD** | `172.10.1.10` | Windows Server | Active Directory / DNS / Usuarios |
| **Srv-Docker** | `192.168.50.10` | Linux (Docker) | InfluxDB / Chronograf |
| **AWS BBDD** | `Endpoint URL` | Managed SQL | Base de Datos Externa |

---

## Enlaces a recursos de la unidad

- [Documentos de la unidad](./documentos/)
- [Diagramas e imágenes](./img/)

## Bibliografía / Webgrafía
- Autor1, Título del libro o artículo, Editorial/Año.
- Sitio web oficial: [Enlace](https://www.ejemplo.com)
- Tutoriales y guías recomendadas: [Enlace](https://www.ejemplo2.com)
