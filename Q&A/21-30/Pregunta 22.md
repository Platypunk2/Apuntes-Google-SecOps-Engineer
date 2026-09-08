![[Pasted image 20260908174524.png]]

**Qué pide el escenario**

- Detectar un **patrón repetitivo de intentos de login SSH por fuerza bruta** hacia una imagen de Compute Engine

- Los intentos **no llegaron a tener éxito** (es decir, buscas el patrón de intentos fallidos/repetidos, no una sesión ya autenticada)

- Necesitas visibilidad **minimizando el impacto en tu cuota de ingesta** (ingestion quota) — es decir, la opción más **liviana/eficiente en volumen de datos**

---

**Por qué A es correcto**

- Los **VPC Flow Logs** capturan **metadatos de las conexiones de red** (IP origen, IP destino, puerto, protocolo, bytes, número de paquetes, etc.) — sin necesidad de inspeccionar el contenido/payload del tráfico.

- Un ataque de fuerza bruta SSH se manifiesta como **múltiples intentos de conexión repetidos hacia el puerto 22** desde la(s) misma(s) IP(s) de origen en un periodo corto — este patrón es perfectamente visible **a nivel de metadatos de conexión**, sin necesitar inspección profunda de paquetes.

- Al ser **solo metadatos** (no el contenido completo del tráfico), los Flow Logs son **mucho más livianos en volumen** comparados con soluciones de inspección profunda — cumpliendo el requisito de **minimizar el impacto en la cuota de ingesta**.

- Además, es una fuente **nativa de GCP a nivel de red**, sin necesitar agentes adicionales dentro de la VM ni licenciamiento extra.

---

**Por qué no las demás**

**B** (Security Command Center Premium findings):

- SCC Premium es un **nivel de servicio (tier) con costo adicional** que agrega y analiza señales de múltiples fuentes para generar _findings_ — es una capa de **análisis derivado**, no la fuente de datos más eficiente o directa para este caso específico.

- Consumir esta ruta implica mayor overhead y costo, sin necesariamente ser más eficiente que ir directo a la fuente de red (Flow Logs) para este patrón específico.

**C** (Cloud IDS logs):

- **Cloud IDS** hace **inspección profunda de paquetes (deep packet inspection)** para detectar amenazas más sofisticadas (malware, exploits, exfiltración) — es una herramienta **mucho más pesada en recursos y costo**, diseñada para amenazas que requieren analizar el contenido del tráfico, no solo su metadata.

- Para un patrón relativamente simple como "muchos intentos de conexión SSH repetidos", usar Cloud IDS sería **sobre-ingeniería**: consume más cuota/recursos de los necesarios para el objetivo planteado.

**D** (Cloud Audit Logs):

- Los **Cloud Audit Logs** registran **actividad administrativa y llamadas a la API de Google Cloud** (quién hizo qué cambio de configuración, quién llamó a qué servicio de GCP) — **no** capturan intentos de login SSH al sistema operativo invitado (guest OS) de una VM.

- Los intentos de login SSH ocurren **dentro del sistema operativo de la VM**, a nivel de red/aplicación — esto está completamente fuera del alcance de lo que registran los Audit Logs, que se centran en el **plano de control de GCP**, no en el tráfico de red hacia la VM.

---

**La idea clave para el examen**

Cuando el escenario pida detectar **patrones de conexión de red repetidos** (como fuerza bruta SSH) **minimizando el volumen/costo de ingesta**, la respuesta casi siempre apunta a **VPC Flow Logs** — porque capturan exactamente el patrón necesario (conexiones repetidas a un puerto específico) usando solo metadatos, siendo mucho más eficientes que herramientas de inspección profunda (Cloud IDS) o servicios de análisis agregado con costo premium (SCC Premium), y a diferencia de Cloud Audit Logs, sí cubren el tráfico de red hacia la VM en lugar de solo la actividad administrativa de GCP.