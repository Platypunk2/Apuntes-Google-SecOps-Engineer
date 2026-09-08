![[Pasted image 20260908114207.png]]

**Qué pide el escenario**

- Fuente de log: una base de datos **MySQL on-premise** (dentro de tu propia infraestructura, no en la nube)

- Objetivo: ingerir esos logs a Google SecOps con el **mínimo esfuerzo posible**

---

**Por qué C es correcta**

- El **Forwarder de Google SecOps** es el componente **nativo y estándar** diseñado específicamente para este caso de uso: recolectar logs desde **fuentes on-premises** (archivos de log, syslog streams, base de datos, etc.) dentro de la red del cliente, y **transmitirlos de forma segura** (con compresión y batching) hacia la instancia de SecOps.

- Es un software ligero (disponible como binario para Linux o contenedor Docker) que se despliega **dentro de tu propia red**, apuntando su configuración al archivo de log de MySQL o al stream de syslog correspondiente.

- Es la **ruta de ingesta más directa** para telemetría on-premises — exactamente lo que pide el enunciado al buscar minimizar el esfuerzo.

---
**Por qué no las demás**

**A (Third-party API feed):**
- Los **feeds** en Google SecOps están diseñados para **datos estructurados** como inteligencia de amenazas o alertas de servicios externos con API — **no** para telemetría cruda de logs provenientes de una base de datos. Es el mecanismo equivocado para este tipo de fuente.

**B (Ingesta directa desde tu organización de Google Cloud):**
- Esta opción no aplica porque el log source **es on-premises**, no un recurso dentro de tu organización de Google Cloud. La ingesta directa desde GCP asume que el dato ya vive dentro del ecosistema de Google Cloud (por ejemplo, vía Cloud Logging), lo cual no es el caso aquí.

**C (Bindplane collection agent):**
- Aunque Bindplane también es una **solución válida** para recolectar logs (y de hecho Google está migrando hacia Bindplane/OpenTelemetry como el modelo de ingesta oficial a futuro), en el contexto de esta pregunta se considera una **solución de socio externo (third-party)** que puede implicar **configuración o licenciamiento adicional** — no es la herramienta **nativa y de mínimo esfuerzo** provista directamente por Google SecOps para este propósito específico.

---

**Nota importante sobre el contexto temporal**

Vale la pena mencionar que Google está **deprecando el Forwarder legacy** (previsto para enero de 2027), migrando hacia **Bindplane + OpenTelemetry** como el modelo de ingesta oficial y recomendado a futuro. Es decir, en el mundo real esto está cambiando — pero para efectos de este examen (que probablemente refleja el estado "clásico" de la certificación), la respuesta esperada sigue siendo el **Forwarder** como la solución nativa de menor esfuerzo.

---

**La idea clave para el examen**

Cuando el escenario mencione **"log source on-premises"** + **"minimize effort"**, la respuesta suele apuntar al **Google SecOps Forwarder** como la herramienta nativa diseñada específicamente para ese propósito — a diferencia de feeds (datos estructurados de terceros), ingesta directa de GCP (solo para recursos ya en la nube de Google), o Bindplane (agente de un socio externo, con overhead adicional de configuración/licenciamiento).
