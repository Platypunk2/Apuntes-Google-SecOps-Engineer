CMDB -> base de datos de gestión de la configuración

Una base de datos de gestión de la configuración (CMDB) almacena información sobre tus activos de TI y cómo se relacionan entre sí para que los equipos puedan entender el impacto y los riesgos que suponen los cambios antes de aplicarlos.

**CMDB (Configuration Management Database)** es un repositorio que almacena información sobre los **activos de TI de una organización** y sus **relaciones/dependencias** — puede incluir hardware, software, servidores, aplicaciones, redes, tanto en la nube como **on-premise**, y también metadatos de negocio (propietario, criticidad, departamento, valor para el negocio).

---
## Comparación CMDB vs CAI



|                     | CMDB                                                                                                          | Cloud Asset Inventory (CAI)                                                                                                                     |
| ------------------- | ------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| Alcance             | Toda la organización: on-prem, multi-cloud, dispositivos de red, aplicaciones, incluso activos "no técnicos"  | Específicamente recursos **dentro de Google Cloud**                                                                                             |
| Quién lo gestiona   | Herramientas de terceros (ServiceNow, BMC Helix, Device42) o procesos internos de ITSM                        | Nativo de GCP, gestionado automáticamente por Google                                                                                            |
| Cómo se llena       | Manualmente, por descubrimiento automatizado, o integraciones — a menudo requiere mantenimiento humano activo | Automático — se actualiza solo, reflejando el estado real de tus recursos en GCP                                                                |
| Contexto de negocio | Sí — incluye criticidad, propietario, valor para el negocio (por eso se usa para "HVA" en la pregunta 14)     | No tanto — es más técnico (configuración, metadatos, relaciones), no necesariamente clasifica "qué tan crítico es este recurso para el negocio" |
| Propósito principal | ITSM (gestión de servicios de TI), gestión de cambios, gestión de incidentes, y contexto de seguridad         | Auditoría, compliance, visibilidad de seguridad, análisis de relaciones entre recursos de GCP                                                   |

#### El punto clave de similitud

Ambos responden a la pregunta **"¿qué tengo y cómo está conectado?"** — son fuentes de **contexto sobre activos**. Por eso en varias preguntas del examen aparecen en roles parecidos: dar contexto adicional a las herramientas de seguridad (SCC o SecOps) para tomar mejores decisiones.

#### La diferencia clave que importa para el examen

- **CMDB** = fuente de verdad más **amplia y orientada al negocio** (incluye criticidad/valor del activo) → por eso en la pregunta 14, se usa el CMDB para obtener **HVA (High-Value Assets)** — ese dato de "qué tan importante es este activo para la empresa" normalmente vive en el CMDB, no en CAI.
- **CAI** = fuente de verdad **específica de GCP**, centrada en configuración técnica y relaciones entre recursos de la nube, sin necesariamente tener ese contexto de "valor de negocio".

#### En la práctica

En muchas organizaciones, ambos coexisten y se **alimentan mutuamente**: podrías tener CAI recolectando el inventario técnico de GCP, y esa información se **sincroniza hacia el CMDB corporativo** para tener una vista unificada de TODOS los activos (cloud + on-prem) con su contexto de negocio completo. Por eso, en el escenario de la pregunta 3 (clasificar IPs internas/externas), C mencionaba explícitamente "hacer lookup contra tu CMDB" como una opción — porque el CMDB sería la fuente más completa y multi-entorno para ese tipo de contexto, mientras que CAI se limitaría solo a lo que existe dentro de GCP.