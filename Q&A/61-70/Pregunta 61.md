![[Pasted image 20260909091437.png]]

#### Qué pide el escenario

- **Confirmar** que ya recibes alertas cuando se crean keys para service accounts **inactivas/no usadas** (dormant)
- **Automatizar la eliminación** de esas keys recién creadas
- **Minimizar el esfuerzo manual**, aprovechando que ya usas **tanto SCC como SecOps** juntos

---

#### Por qué C es correcta

> _"Use the Initial Access: Dormant Service Account Key Created finding from SCC, and ingest this finding into Google SecOps. Create a custom action in Google SecOps SOAR that is triggered on this finding. Use the built-in IDE to build code to delete the service account key."_

- SCC ya tiene un **finding nativo predefinido** llamado exactamente **"Initial Access: Dormant Service Account Key Created"** — es decir, la **detección ya existe**, no necesitas construir ninguna lógica de detección desde cero (esto responde directamente a la parte de "confirm that you are receiving alerts").
- Como tu empresa **ya usa SecOps junto con SCC**, la ruta de **menor esfuerzo** es **ingerir ese finding ya existente** hacia SecOps, y usar el **motor de automatización nativo (SOAR)** para reaccionar a él — creando una **acción personalizada (custom action)** que se dispara automáticamente cuando llega ese finding específico, y que ejecuta el código de eliminación de la key.
- Esto aprovecha **toda la infraestructura y flujo de trabajo que ya tienes configurado** (SCC → SecOps → SOAR), sin necesitar construir **componentes de infraestructura adicionales** (como topics de Pub/Sub o funciones de Cloud Run independientes) — es la integración **más directa y con menos piezas nuevas que mantener**.


---

#### Por qué no A y B (construyen detección desde cero)

**A** (Cloud Logging sink filtrando por `methodName` + Cloud Run function):

- Esto **ignora completamente** que SCC **ya tiene un finding nativo** para exactamente este escenario. Construir tu propio filtro de logging y pipeline de Pub/Sub + Cloud Run es **reinventar la rueda** — mucho más esfuerzo de desarrollo y mantenimiento que simplemente **usar el finding que ya existe**.

**B** (Regla YARA-L personalizada + custom action en SOAR):

- Mismo problema: **construir una regla YARA-L desde cero** para detectar esto es innecesario, cuando SCC **ya genera esta alerta de forma nativa**. Ignora la detección ya disponible y duplica esfuerzo de desarrollo sin necesidad.

#### Por qué no D (mismo finding, pero arquitectura más compleja)

> _"Use the Initial Access: Dormant Service Account Key Created finding from SCC, and write this finding to a Pub/Sub topic. Create a Cloud Run function that subscribes to the Pub/Sub topic and deletes the service account key."_

- Esta opción sí usa **correctamente el finding nativo** (a diferencia de A y B) — hasta ahí, buen punto de partida.
- Pero la diferencia con C está en **dónde construyes la remediación**: D te lleva a **salir del ecosistema de SecOps/SOAR** y construir una **arquitectura paralela** (Pub/Sub + Cloud Run) para manejar la respuesta — esto significa **mantener infraestructura adicional separada** (topics, funciones serverless, permisos IAM adicionales, monitoreo de esa función, etc.).
- Como la empresa **ya tiene SecOps SOAR** (con su motor de automatización, IDE integrado, y capacidad de manejar acciones personalizadas), es **menos esfuerzo** quedarte **dentro de esa misma plataforma** (opción C) que construir y mantener una **arquitectura de eventos completamente separada** en Google Cloud (opción D) para lograr lo mismo.

---

#### La idea clave para el examen

Cuando el escenario mencione que la organización **ya usa tanto SCC como SecOps** y pida **minimizar esfuerzo manual**, la respuesta correcta casi siempre combina: **(1) aprovechar un finding/detección nativa ya existente** (en lugar de construir detección desde cero) **+ (2) mantener la remediación dentro del mismo ecosistema de SecOps/SOAR** ya establecido (en lugar de construir una arquitectura paralela con servicios adicionales de GCP como Pub/Sub y Cloud Run), ya que reutilizar la plataforma existente siempre implica menos piezas nuevas que desarrollar y mantener.