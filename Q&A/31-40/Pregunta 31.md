![[Pasted image 20260908213211.png]]

#### Qué pide el escenario

- Una **identidad externa** con un rol IAM **altamente privilegiado** en un proyecto crítico de producción
- No se sabe **cuánto tiempo** ha tenido acceso
- El objetivo es determinar **qué acciones realizó** esta identidad en tu entorno (no solo qué _podría_ haber hecho)
- Ya tienes disponibles: **logs centralizados en Cloud Logging** + **exportación histórica a BigQuery**

---

#### Por qué D es correcta

> _"Execute queries against the centralized Cloud Logging bucket and the BigQuery dataset to filter for logs where the principal email matches the external identity."_

- La pregunta clave del escenario es **"¿qué acciones fueron tomadas por esta identidad?"** — eso es exactamente lo que registran los **Cloud Audit Logs**: cada llamada a la API de GCP incluye el campo `protoPayload.authenticationInfo.principalEmail`, identificando **quién** hizo **qué** acción.
- Al filtrar directamente por el **email de la identidad externa sospechosa** contra los logs ya centralizados (tanto en Cloud Logging como en el histórico ya exportado a BigQuery), obtienes **evidencia directa y específica** de cada acción que esa identidad ejecutó — sin importar cuánto tiempo ha tenido acceso, porque estás consultando **tanto los datos recientes como los históricos** ya disponibles.
- Es la solución **más directa**: vas exactamente a la fuente de verdad (los logs de auditoría) y filtras por el identificador preciso de la entidad sospechosa (su email), usando la infraestructura de logging **que ya está lista y disponible** (sin necesitar configurar nada nuevo).

---

#### Por qué no las demás

**A** (Policy Analyzer para identificar recursos accesibles + examinar logs relacionados):

- **Policy Analyzer** te dice **qué recursos podría acceder** esa identidad según sus permisos actuales — es decir, te da el **alcance potencial** (qué podría hacer), no evidencia de **qué hizo realmente**. Es un paso adicional/indirecto: primero identificar recursos, y luego revisar logs "relacionados a esos recursos" (que podría incluir actividad de muchas otras identidades también, generando más ruido). Es menos preciso y más indirecto que simplemente filtrar directamente por el email de la identidad sospechosa, que es información que ya tienes.

**B** (VPC Flow Logs correlacionando IPs con eventos de login):

- Los **VPC Flow Logs** capturan metadatos de **tráfico de red** (conexiones, IPs, puertos) — son útiles para analizar comunicación de red, pero **no registran acciones de IAM/API** como creación de recursos, cambios de configuración, o accesos a datos a través de la consola/API de GCP. No es la fuente correcta para rastrear **acciones administrativas** de una identidad con rol IAM privilegiado.

**C** (IAM Recommender insights + SCC findings):

- **IAM Recommender** sugiere **optimizaciones de permisos** (por ejemplo, "este rol tiene permisos que no se usan, considera reducirlo") — es una herramienta de **higiene/postura de IAM**, no de **investigación forense de actividad histórica específica**.
- **SCC findings** te alertaría sobre **configuraciones riesgosas** (como la existencia de esta identidad externa privilegiada en sí), pero no te da un registro detallado de **cada acción específica** que la identidad ejecutó — es más una señal de "esto es riesgoso" que una fuente de evidencia de "esto es lo que pasó."

---

#### La idea clave para el examen

Cuando el objetivo sea determinar **qué acciones concretas realizó una identidad específica** (no qué podría hacer, ni señales de riesgo generales), la fuente de verdad más directa en GCP son los **Cloud Audit Logs**, filtrando por el campo de **principal/identidad** — ya sea en Cloud Logging (reciente) o en su exportación histórica a BigQuery (para cubrir el rango de tiempo desconocido). Herramientas como Policy Analyzer, IAM Recommender o SCC dan **contexto de riesgo/alcance potencial**, pero no son la fuente **forense directa de qué ocurrió realmente**.