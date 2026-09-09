![[Pasted image 20260908211012.png]]

#### Qué pide el escenario

- Las VMs tienen **IPs públicas** — riesgo de seguridad porque quedan expuestas directamente a internet (superficie de ataque)
- Pero necesitan **comunicarse con sitios externos** como parte de su operación normal (tráfico **saliente**, no entrante)
- Necesitas **reducir la necesidad de IPs públicas** sin romper esa funcionalidad

---

#### Por qué B es correcta

> _"Cloud NAT"_

- **Cloud NAT (Network Address Translation)** permite que las VMs **sin IP pública** (solo con IP privada/interna) puedan **iniciar conexiones salientes a internet** — el servicio traduce el tráfico saliente a través de un rango de IPs públicas gestionadas centralmente, sin necesidad de asignar una IP pública a cada VM individual.
- Esto resuelve exactamente el problema descrito: las VMs siguen pudiendo comunicarse con sitios externos (tráfico saliente), pero **ya no necesitan tener su propia IP pública individual** — reduciendo drásticamente la superficie de ataque, porque las VMs **ya no son directamente alcanzables desde internet** (no hay tráfico entrante posible sin una IP pública asociada).
- Es la solución **estándar y recomendada** en GCP para este patrón: "necesito salida a internet, pero no necesito ni quiero exposición entrante."

---

#### Por qué no las demás

**A** (Google Cloud Armor):

- Cloud Armor es un servicio de **protección perimetral** (WAF/DDoS) que filtra y protege el tráfico **entrante** hacia recursos expuestos (como balanceadores de carga) — no elimina la necesidad de tener una IP pública, de hecho se usa típicamente **junto con** recursos que sí exponen IPs públicas. No resuelve el objetivo de **reducir** la necesidad de IPs públicas.

**C** (Cloud Router):

- Cloud Router se usa principalmente para **intercambio dinámico de rutas (BGP)**, típicamente en combinación con **Cloud VPN** o **Interconnect** para conectividad híbrida entre tu red on-premises y GCP. No está diseñado para permitir que las VMs tengan salida a internet sin IP pública — de hecho, **Cloud NAT depende de un Cloud Router** para funcionar, pero el Router por sí solo no resuelve el problema planteado.

**D** (Cloud VPN):

- Cloud VPN conecta tu red **on-premises** con tu VPC de GCP de forma segura (o VPC a VPC) — es para conectividad **híbrida/privada entre redes**, no para dar salida a internet a VMs que necesitan comunicarse con **sitios externos públicos** (como APIs de terceros, actualizaciones de software, etc.). No aplica al caso de uso descrito.

---

#### La idea clave para el examen

Cuando el escenario describa **"VMs necesitan tráfico saliente a internet, pero queremos eliminar/reducir IPs públicas"** (para minimizar la superficie de ataque expuesta a conexiones entrantes), la respuesta estándar en GCP es **Cloud NAT** — permite salida a internet sin exposición entrante, a diferencia de Cloud Armor (protección de tráfico entrante), Cloud Router (enrutamiento dinámico, normalmente para conectividad híbrida) o Cloud VPN (conectividad privada entre redes, no acceso a internet público).