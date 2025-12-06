[Volver al índice general](../README.md)

# UD1 – Análisis del entorno y detección de necesidades tecnológicas

## Índice de apartados

- [ ] **[1. Análisis del sector tecnológico](#1-análisis-del-sector-tecnológico)**
- [ ] **[2. Selección de la empresa o contexto de trabajo](#2-selección-de-la-empresa)**
- [ ] **[3. Identificación de necesidades tecnológicas](#3-identificación-de-necesidades-tecnológicas)**
- [ ] **[4. Oportunidades y viabilidad del proyecto](#4-oportunidades-y-viabilidad-del-proyecto)**
- [ ] **[5. Obligaciones legales y normativas](#5-obligaciones-legales-y-normativas)**
- [ ] **[6. Guion inicial del proyecto](#6-guión-inicial-del-proyecto)**

---

## Apartados

### [1. Análisis del sector tecnológico](#índice-de-apartados)
Voy a centrar mi análisis en Mairena del Aljarafe, ya que he vivido toda mi vida allí, así que hablaré del *Parque Industrial de Servicios del Aljarafe (PISA)*, un centro empresarial estratégico que alberga una gran concentración de compañías de servicios, ingeniería y consultoría que requieren una infraestructura informática avanzada y segura...pero antes, hablemos un poco de España profundizando en STEM.

El [informe](/documentos/stem.pdf) del SEPE establece que las ocupaciones STEM (Ciencia, Tecnología, Ingeniería y Matemáticas) son el motor del crecimiento económico y la digitalización en España. En este informe observamos que:

- *Tendencia de Empleo*: La evolución de la población ocupada en STEM es positiva. Entre el cuarto trimestre de 2019 y el cuarto trimestre de 2023, el empleo STEM aumentó un 25,60%. Además, a diferencia de otras ocupaciones, el sector no se vio afectado por la crisis económica de 2020.

- *Estabilidad Contractual*: El sector STEM ofrece mayor estabilidad laboral que el mercado general, con una media de 1,46 contratos por persona contratada en 2023, frente a los 2,26 de la media estatal. Además, el **54,31% de los contratos en ocupaciones STEM son de carácter indefinido**.

Ahora sí, teniendo en visión, el crecimiento que observamos en España de manera general en el sector tecnológico, centrémonos en el PISA.

El PISA funciona como un Hub Tecnológico Regional en el área metropolitana de Sevilla, albergando aproximadamente más de **850 empresas** y generando más de **10.000 puestos de trabajo** (según datos de la página del [PISA](https://parquepisa.org/)) e incluso podríamos hablar de sostenibilidad ya que ha invertido casi 1 millón de euros en carriles bici y aceras para poder ir a pie desde Mairena del Aljarafe, Bormujos o Tomares.

Al estar estudiando ASIR, voy a centrarme en las empresas que más administradores de sistemas tienen contratados y pueden seguir contratando:

- **Servinform**: Es una de las grandes empresas con presencia en PISA, especializada en servicios de Transformación Digital, Automatización Inteligente (RPA e IA), Customer Experience y Process Outsourcing. Una compañía de este calibre requiere constantemente administradores de sistemas expertos en Alta Disponibilidad (SAD), Infraestructura Cloud y seguridad de datos para gestionar sus servicios a terceros (Banca, Seguros, IT, Telco).

- **Avanza Sistemas**: Dedicada a los sistemas de información y servicios tecnológicos. Su participación en programas como el Kit Digital y Kit Consulting la posiciona como un proveedor clave para la digitalización de pymes, lo que implica una necesidad constante de implementar y mantener servidores, redes y bases de datos.

- **Teamwork Solutions**: Es una empresa de tecnología y consultoría con una fuerte presencia local, enfocada en la provisión de soluciones tecnológicas dinámicas e integradas para la gestión empresarial. Ésta será la empresa donde haré las prácticas de segundo año de ASIR. 

---

### [2. Selección de la empresa](#índice-de-apartados)

### Empresa seleccionada -> Teamwork Solutions (TWS).

Es una empresa de Consultoría y Soluciones Tecnológicas ubicada en el Polígono PISA de Mairena del Aljarafe (Sevilla), por su alta dependencia de la estabilidad, seguridad y eficiencia de sus sistemas de gestión (ERP).

TWS se enfoca en el desarrollo y soporte de software de gestión integral para empresas, abarcando áreas sensibles como la facturación, contabilidad y recursos humanos. Por lo tanto, TWS no solo opera en el sector de Información y Comunicaciones, que concentra el mayor volumen de contratación para Técnicos TIC a nivel nacional, sino que además maneja datos críticos, lo que exige los más altos estándares de sistemas, además de cumplir con las normativas y leyes necesarias como la Protección de Datos (RGPD y LOPDGDD) o la ENS (Esquema Nacional de Seguridad).

---

### [3. Identificación de necesidades tecnológicas](#índice-de-apartados)

Según lo que me han comentado compañeros que estuvieron alli el año pasado haciendo las prácticas, lo que la empresa necesitó en su momento, que no sé si lo habrán resuelto, es la automatización del proceso para lanzar el software suyo propio, que lo tienen que ir haciendo a mano.

Gracias a mi formación en ASIR, creo que es uno de mis puntos fuertes, y que se puede lograr automatizar ese proceso con un script que lance su software y que además dé el nombre de usuario y contraseña a la empresa en cuestión, enlazando el software con la base de datos de Teamwork Solutions. Además, me comentaron que lo estaban haciendo en PowerShell, es decir, para Windows. Creo que estaría bien tener el mismo script enfocado a Linux por lo que pueda pasar. 

Incluso otra manera de automatizarlo todo y acelerar el despliegue del software, puede ser utilizando Docker. Con ello lograría un rápido despliegue además de una portabilidad mucho mayor que tener que estar instalando y descargando en los equipos de manera manual.

---

### [4. Oportunidades y viabilidad del proyecto](#índice-de-apartados)

Teniendo en cuenta de las necesidades de la empresa, la viabilidad es bastante alta, ya que lo novedoso o necesario no requiere una inversión monetaria, si no que todo se puede realizar con softwares de código abierto y totalmente gratuitos, como es Docker. Lo necesario sería tener contratado a alguien que tengas dichos conocimientos, pero al fin y al cabo, es un trabajador más de la empresa, por lo que, teniendo una visión a corto, medio y largo plazo, le saldría rentable para la empresa.

Si implementásemos dicha solución, incluso se podría monitorizar todo el rendimiento en tiempo real, con Grafana y Prometheus, que siguen siendo de código abierto y gratis de usar.

---

### [5. Obligaciones legales y normativas](#índice-de-apartados)

Al trabajar con sistemas de gestión que manejan datos sensibles, es imprescindible considerar el marco legal que rige en España y la Unión Europea.

Normativa de Cumplimiento Crítico:

- Reglamento General de Protección de Datos (RGPD / GDPR):
  - Relevancia: TWS maneja datos de clientes (identificativos, facturación). El proyecto debe asegurar que los sistemas de Alta Disponibilidad y Backup contemplan la privacidad desde el diseño y la seguridad por defecto.

- Esquema Nacional de Seguridad (ENS) (Real Decreto 311/2022):
  - Relevancia: Si TWS trabaja (o aspira a trabajar) con la Administración Pública o maneja información clasificada como de seguridad media/alta, debe cumplir con los requisitos del ENS.

- Ley de Propiedad Intelectual (LPI):
  - Relevancia: El software desarrollado por TWS es su activo principal.
 
---

### [6. Guión inicial del proyecto](#índice-de-apartados)

---

## Enlaces a recursos de la unidad

- [Documentos de la unidad](./documentos/)
- [Diagramas e imágenes](./img/)

  ## Bibliografía / Webgrafía 
- Autor1, Título del libro o artículo, Editorial/Año.
- Sitio web oficial: [Enlace](https://www.ejemplo.com)
- Tutoriales y guías recomendadas: [Enlace](https://www.ejemplo2.com)

