![[Pasted image 20260906225359.png]]

**Lo que pide el escenario:**

1. Estás haciendo **threat hunting proactivo** (buscar activamente, no reaccionando a una alerta puntual)
2. La hipótesis es **movimiento lateral:** de un cluster GKE de desarrollo hacia sistemas críticos de producción
3. Necesitas: **identificar IoCs** y **priorizar acciones** usando **herramientas de seguridad de Google Cloud** — antes de meterte a analizar logs crudos uno por uno
4. La palabra clave es **"before analyzing raw logs in detail"** -> te están pidiendo el paso lógico anterior, no el análisis exhaustivo final.

---

La respuesta correcta es la **A**, ya que:

Esta opción cubre exactamente lo que se pide, en el orden correcta:

1. **Filtrar por el cluster en SCC** -> te da una vista centralizada de todos los *findings* (hallazgos) relacionados con ese recurso específico, sin tener que ir a buscar logs individuales.

2. **Timeline agregado de findings** -> te permite ver la secuencia de eventos sospechosos de forma cronológica, ideal para reconstruir cómo se movió al atacante.

3. **Attack path simulation + exposure scores** -> esta es la pieza clave: SCC Enterprise puede **simular rutas de ataque** desde un recurso comprometido hacia recursos críticos, y darte un **score de exposición** que te dice qué camino es más peligroso o más probable. Esto responde directamente a la sospecha de movimiento lateral hacia producción, y te dice **qué priorizar primero**.

Es decir: A te da **visibilidad + contexto de riesgo priorizado**, usando herramientas nativas de SCC, sin tener que sumergirse todavía en logs crudos.

---

**Por qué no las demás**

**B** — Revisar threat intel feeds y enriquecer con TTPs/campañas:

- Es útil como **contexto adicional**, pero no es una acción de **investigación activa sobre el incidente específico**. No te dice nada sobre que está pasando en *tu cluster* ahora mismo, ni te ayuda a priorizar acciones concretas sobre el movimiento lateral sospechado.

**C** — Buscar VM Threat Detection findings, enfocarse en malware/rootkits en los nodos:

- Es demasiado **específico y limitado:** solo cubre un tipo de amenaza (malware/rootkits a nivevl de VM/nodo).

- El escenario habla de **credenciales comprometidas y movimiento lateral,** que es un problema de **identidad y conexiones de red**, no necesariamente de malware en el sistema operativo del nodo. Estarías buscando en el lugar equivocado.

**D** — Crear un playbook de SOAR que aísle automáticamente y alerte:

- Esto es una **acción de respuesta/remediación automática,** no de **investigación/threat hunting.**

- El enunciado pide identificar IoCs y priorizar investigación — es decir, estás en fase de **análisis**, no todavía en fase de **contención automática**. Aplicar una respuesta automática antes de confirmar qué está pasando podría ser prematuro (falsos positivos, interrupciones innecesarias).

---
**La idea central para recordar**

Cuando la pregunta dice **"threat hunting"** + **"orioritize before deep log analysis"**, casi siempre la respuesta correcta usa la **vista agregada de SCC con attack path simulation** — es la herramienta diseñada específicamente para darte una imagen de alto nivel de riesgo y rutas de ataque antes de bajar al detalle.