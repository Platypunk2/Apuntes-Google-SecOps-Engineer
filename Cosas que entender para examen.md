**Unidad 1 – Platform Operations (~14%)** 

- Priorización de fuentes de telemetría (SCC, SecOps, GTI, Cloud IDS)
- Integración de múltiples herramientas en la arquitectura de seguridad
- Configuración de autenticación/autorización (IAM roles y permisos) para SCC y SecOps
- Cloud Audit Logs y data access logs
- Configuración de acceso API para automatizaciones (service accounts, API keys)
- Workforce Identity Federation

**Unidad 2 – Data Management (~14%)** 

- Enfoques de ingesta de datos en SCC y SecOps
- Parsers: evaluación, modificación y extensión en Google SecOps
- Normalización de datos (UDM) desde distintas fuentes de log
- Labels de ingesta y gestión de costos
- Ya visto: diferenciación de datos de evento vs. entidad, aliasing fields para enrichment (Entity Context Graph)

**Unidad 3 – Threat Hunting (~19%)** 

- Desarrollo de queries para buscar actividad anómala
- Análisis de comportamiento de usuario para detectar anomalías
- Investigación de red/endpoints/servicios con Logs Explorer, Log Analytics, BigQuery
- Colaboración con el equipo de IR para identificar amenazas activas
- Desarrollo de hipótesis basadas en threat intel, postura e incidentes
- Búsqueda de IOCs en logs históricos
- Retrohunt de datos históricos con logs recién enriquecidos
- Análisis de entity risk score
[[Qué es Risk Analytics]]

**Unidad 4 – Detection Engineering (~22%, la de mayor peso)** 

- Sintaxis YARA-L completa (estructura, variables, entities, reference lists, data tables, operadores/modificadores)
- Reconciliar threat intelligence con actividad de usuarios/activos
- Reglas de detección usando risk values y Google SecOps Risk Analytics
- Reglas para detectar cambios de postura/riesgo (SCC Security Health Analytics, posture management)
- Identificar procesos/dominios/IPs de baja prevalencia (dashboards + YARA-L)
- Configuración de SCC Event Threat Detection custom detectors para IOCs
- Scoring de alertas según riesgo de IOCs, y reducción de falsos positivos

**Unidad 5 – Incident Response (~21%)

- Contención e investigación de incidentes: evidencia forense, análisis de alcance
- Análisis forense de artefactos (Hash, IP, URL, Binaries) con GTI
- Root cause analysis con SCC / SecOps SIEM
- Playbooks de respuesta: automatización, priorización de enriquecimientos, integraciones (SOAR)
- Ciclo de vida de gestión de casos: etapas, escalación, handoffs

**Unidad 6 – Observability (~10%)** 

- Dashboards y reportes (SecOps SOAR/SIEM, Looker Studio)
- Métricas/KPIs de seguridad clave
- Monitoreo de salud: alertas con umbrales, Cloud Monitoring, Cloud Logging
- Configuración de silent source detection

**Unidad 7 – YARA-L Language (transversal a Unidades 3 y 4)**

- Estructura de reglas, variables, placeholders, match, condition
- Event vs. Entity, Entity Context Graph
- Reference lists vs. Data tables
- Operadores (AND/OR/NOT) y modificadores (nocase, any/all)
- Joins entre múltiples eventos

[[Que es YARA-L]]

**Unidad 8 – SIEM/SOAR/GTI integrado (transversal a Unidades 1, 3, 4 y 5)**

- Cómo se conectan Chronicle (SIEM), ex-Siemplify (SOAR) y GTI en un solo flujo
- Cómo se combinan con Security Command Center

---

# Preguntas para practicar

[[Pregunta 1]]
[[Pregunta 2]]
[[Pregunta 3]]
