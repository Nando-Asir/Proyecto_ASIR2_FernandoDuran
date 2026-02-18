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

El diseño sigue un **modelo de defensa** en profundidad:

- **RED 0 (DMZ - 172.15.0.0)**: Zona desmilitarizada que actúa como frontera. Aquí se sitúa el servidor Linux que gestiona las peticiones externas antes de conectar con el backend.
- **RED 1 (Active Directory - 172.10.1.0)**: Red interna dedicada a la gestión de identidades y políticas de grupo mediante Windows Server.
- **RED 2 (Monitoreo - 192.168.50.0)**: Red aislada para el despliegue de contenedores (Docker) encargados del análisis de logs y métricas del sistema.
- **Conectividad Cloud**: El servidor Linux establece una conexión segura con AWS BBDD para la persistencia de datos.

---

### [3. Definición de objetivos y fases del proyecto](#índice-de-apartados)

1. **Fase 1 Networking**: Configuración de interfaces de red y reglas de enrutamiento entre las tres subredes.
2. **Fase 2 Servicios Core**: Despliegue del controlador de dominio (AD) y configuración del servidor Linux.
3. **Fase 3 Contenerización**: Implementación de la pila de monitoreo en Docker.
4. **Fase 4 Integración Cloud**: Configuración de conectividad y seguridad (Security Groups) con AWS.

---

### [4. Recursos y presupuesto](#índice-de-apartados)

- **Hardware**: Ordenador con máquinas virtuales (aún por ver dónde las creo).
- **Software**: OPNsense (firewall), WireGuard (VPN), Debian (servidor), Windows Server con AD (permisos y usuarios), Docker (monitorización con InfluxDB y Chronograf) y Telegraf (parecido a un exporter).
- **Cloud**: Uso de AWS para un servicio posterior.
- **Costo Estimado**: Mínimo (uso de software Open Source y capas gratuitas de Cloud).

---

### [5. Documentación técnica](#índice-de-apartados)

- Tabla de Direccionamiento \
| Interfaz Red | Segmento IP | Máscara | Puerta de Enlace (GW) | Descripción |
|---|---|---|---|---|
| RED 0 (DMZ) | 172.15.0.0/24 | 255.255.255.0 | 172.15.0.1 | Zona perimetral y Servidor Linux |
| RED 1 (AD) | 172.10.1.0/24 | 255.255.255.0 | 172.10.1.1 | Gestión de Active Directory (Windows) |
| RED 2 (Monitor) | 192.168.50.0/24 | 255.255.255.0 | 192.168.50.1 | Servicios de Monitoreo y Docker |
| WAN, DHCP | Static | N/A | ISP Gateway | Salida a Internet y Conexión AWS |

---

## Enlaces a recursos de la unidad

- [Documentos de la unidad](./documentos/)
- [Diagramas e imágenes](./img/)

## Bibliografía / Webgrafía
- Autor1, Título del libro o artículo, Editorial/Año.
- Sitio web oficial: [Enlace](https://www.ejemplo.com)
- Tutoriales y guías recomendadas: [Enlace](https://www.ejemplo2.com)
