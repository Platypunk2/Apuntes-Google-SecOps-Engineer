![[Pasted image 20260908231615.png]]

#### Qué pide el escenario

1. Usas **SCC pero NO SecOps** — importante, porque descarta cualquier solución que dependa de YARA-L, UDM, o el motor de detección de SecOps
2. Necesitas **windowing y agregación de datos** sobre un período de tiempo (patrones de comportamiento, no eventos aislados) — esto sugiere análisis a **escala** (miles de proyectos)
3. Necesitas basar detecciones en **eventos de log específicos** O en **cálculos avanzados** (agregaciones, matemáticas más complejas)
4. Necesitas una **interfaz para que los analistas hagan triage** de las alertas — esto apunta directamente a que el resultado final debe integrarse como **findings dentro de SCC**, que es la interfaz de triage disponible ya que no tienes SecOps

---

#### Por qué B es correcta

> _"Sink the logs to BigQuery, and configure Cloud Run functions to execute a periodic job and generate normalized alerts in a Pub/Sub topic for findings. Use log-based metrics to generate event-driven alerts and send these alerts to the Pub/Sub topic. Write the alerts as findings using the SCC API."_

- **BigQuery** es la herramienta correcta para **windowing y agregación a escala** — permite hacer consultas SQL complejas (ventanas de tiempo, cálculos avanzados, joins entre miles de proyectos) sobre grandes volúmenes de logs, algo que sería inmanejable de forma eficiente con soluciones más simples.
- Combina **dos mecanismos de detección** según el tipo de patrón necesario:
    - **Cloud Run + jobs periódicos** contra BigQuery → para detecciones que requieren **cálculos avanzados/agregaciones complejas** (el "advanced calculations" del enunciado)
    - **Log-based metrics** → para detecciones más simples basadas en **eventos específicos de log** (el "specific log events" del enunciado)
- Ambos caminos convergen hacia un **Pub/Sub topic**, normalizando las alertas antes de escribirlas.
- El paso final — **escribir los resultados como findings usando la SCC API** — es la pieza clave: esto te da la **interfaz de triage para analistas** que pide el enunciado, aprovechando la consola nativa de SCC (que ya tienes) en lugar de construir una UI personalizada desde cero.

---
#### Por qué no las demás

**A** (Cloud SQL + Cloud Run + Pub/Sub + alertas por email):

- **Cloud SQL** es una base de datos relacional **transaccional**, no está optimizada para análisis agregado a gran escala como BigQuery (que es un data warehouse diseñado justo para este tipo de consultas analíticas masivas). Con "miles de proyectos activos," Cloud SQL sería un cuello de botella de rendimiento.
- Además, termina en **alertas por email** — esto no cumple el requisito de darle a los analistas una **interfaz de triage** estructurada; el email es un canal de notificación, no una herramienta de gestión/triage de alertas.

**C** (Log-based metrics + Cloud Monitoring alert policy + email):

- Los **log-based metrics** por sí solos son buenos para patrones **simples** (contar ocurrencias, umbrales básicos), pero **no soportan bien el windowing/agregación avanzada** que pide el enunciado para "cálculos avanzados" complejos a través de miles de proyectos.
- De nuevo, termina en **alertas por email** vía Cloud Monitoring — no proporciona la interfaz de triage estructurada que necesitan los analistas.

**D** (Log sinks agregados + JSON a Cloud Storage + alertas por "write event"):

- Guardar hallazgos como **archivos JSON en Cloud Storage** no es una solución escalable ni queryable para windowing/agregación — no puedes hacer análisis de series de tiempo o cálculos complejos eficientemente sobre archivos JSON sueltos.
- Generar una alerta simplemente por el **evento de "escritura"** de un archivo es una señal muy básica, no sostiene lógica de detección sofisticada, y tampoco entrega una interfaz de triage adecuada para los analistas.

---
#### La idea clave para el examen

Cuando el escenario necesite **análisis agregado/windowing a gran escala** (miles de proyectos) **sin usar SecOps**, la arquitectura correcta usualmente combina: **BigQuery** (para agregación y cálculos complejos) + **Cloud Run/log-based metrics** (para generar las alertas) + **Pub/Sub** (para normalizar el flujo) + **escritura de resultados como findings vía la SCC API** (para aprovechar la interfaz de triage nativa de SCC, que es la única disponible cuando no tienes SecOps). Cualquier opción que termine solo en **notificaciones por email** falla el requisito de dar una **interfaz de triage** real a los analistas.