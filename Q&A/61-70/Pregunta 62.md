![[Pasted image 20260909091809.png]]

### Qué pide el escenario

Necesitas **aumentar** la implementación existente de Security Command Center (SCC) con **detectores adicionales**. Tienes una **lista propia de IOCs conocidos** (indicadores de compromiso, ej. direcciones IP maliciosas) y quieres incorporar **señales externas** (esos IOCs de tu threat intelligence) a la capacidad de detección, para asegurar **cobertura amplia**.

La clave aquí: quieres que SCC **detecte automáticamente** actividad relacionada con IPs maliciosas específicas que tú conoces — no un framework de compliance, no un módulo genérico de configuración insegura, sino algo enfocado en **coincidencia de IOCs (IPs) contra actividad en tu entorno**.


---

### Por qué A es correcta: módulo personalizado de ETD con plantilla "Configurable Bad IP"

**Event Threat Detection (ETD)** permite crear **módulos personalizados (custom modules)** usando plantillas predefinidas. Una de ellas es específicamente **"Configurable Bad IP"**:

- Esta plantilla está diseñada exactamente para este caso de uso: **cargar tu propia lista de IPs maliciosas conocidas** (tus IOCs) y hacer que ETD las use como criterio de detección.
- ETD analiza el tráfico de red/logs de tu entorno en busca de comunicación con esas IPs específicas — generando findings automáticamente cuando hay coincidencias.
- Es la forma **nativa, integrada y de bajo esfuerzo** de incorporar señales externas (tus IOCs) a las capacidades de detección existentes de SCC.
- Cumple exactamente el objetivo: aumentar la cobertura de detección con información externa (tu lista de IOCs) sin reconstruir infraestructura.

---

### Por qué las otras opciones NO son la mejor opción

**B. Log sink personalizado + SCC API para generar findings manualmente**

- Esto requiere **construir una solución completamente custom**: un sink que filtre por IPs, lógica para comparar contra tu lista, y llamadas a la API de SCC para crear findings uno por uno.
- Es mucho más esfuerzo de ingeniería y mantenimiento comparado con usar una plantilla ya existente diseñada para este propósito exacto.
- Reinventa algo que ETD ya ofrece de forma nativa.

**C. Posture personalizada combinando ETD y SHA prebuilt**

- Una **posture** en SCC se usa principalmente para definir y monitorear **configuraciones de cumplimiento/compliance** (políticas de seguridad deseadas), no para inyectar IOCs externos específicos como IPs maliciosas.
- Combinar detectores prebuilt de ETD y SHA en una posture no te da un mecanismo para cargar tu lista particular de IOCs — no resuelve el problema planteado.

**D. Módulo personalizado de SHA usando el recurso "compute address"**

- **Security Health Analytics (SHA)** está enfocado en detectar **configuraciones erróneas o inseguras** de recursos (ej. reglas de firewall abiertas, buckets públicos), no en hacer matching contra listas de IOCs de amenazas externas.
- Un módulo SHA sobre "compute address" evaluaría configuración de direcciones IP de recursos, no compararía tráfico contra una lista de IPs maliciosas conocidas — no es la herramienta correcta para IOCs de threat intelligence.