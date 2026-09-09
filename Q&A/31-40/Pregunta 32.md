![[Pasted image 20260908214309.png]]

#### Qué pide el escenario

- Detectar cuando un usuario **descarga un volumen de datos inusual comparado con SU PROPIA línea base establecida** (baseline) — no un umbral fijo genérico
- Usar la **menor cantidad de esfuerzo** posible
- Es un requerimiento de **comportamiento anómalo relativo al individuo**, no un límite absoluto igual para todos

---

#### Por qué D es correcta

> _"Enable curated detection rules for User and Endpoint Behavioral Analytics (UEBA), and use the Risk Analytics dashboard to identify metrics associated with the anomalous activity."_

- **UEBA (User and Endpoint Behavioral Analytics)** es una funcionalidad **nativa y curada** de Google SecOps diseñada exactamente para este propósito: establecer automáticamente una **línea base de comportamiento normal por usuario/entidad**, y detectar desviaciones significativas de esa línea base individual — sin que tú tengas que definir manualmente qué constituye "normal" para cada usuario.
- Al **habilitar las reglas curadas de UEBA** (ya construidas y mantenidas por Google) y usar el **Risk Analytics dashboard** para visualizar las métricas de comportamiento anómalo, obtienes justo lo que pide el enunciado — con **el mínimo esfuerzo posible**, porque no tienes que diseñar, programar, ni mantener ninguna lógica de detección tú mismo.
- Es la solución "lista para usar" (out-of-the-box) que resuelve directamente el caso de uso descrito.

---

#### Por qué no C (la opción más "técnica" y tentadora)

> _"Develop a custom YARA-L detection rule that counts download bytes per user per hour and triggers an alert if a threshold is exceeded."_

- El problema de fondo con C es que una regla YARA-L personalizada con un **threshold fijo** (ej: "más de X GB por hora") **no representa una verdadera línea base individual por usuario** — es un umbral **estático y genérico** aplicado a todos por igual.
- Esto no captura bien el matiz de "unusual **compared to the user's established baseline**": un usuario que normalmente descarga poco y de repente descarga el doble ya sería anómalo para él, aunque no supere un umbral fijo genérico; mientras que otro usuario que rutinariamente descarga grandes volúmenes (por su rol) generaría falsos positivos constantes con un umbral fijo.
- Además, requiere **desarrollo, prueba y mantenimiento manual** de la lógica — esto es **significativamente más esfuerzo** que simplemente habilitar una funcionalidad ya curada y lista (UEBA), lo cual contradice el requisito explícito de "**least amount of effort**."

#### Por qué no A

> _"Inspect Security Command Center (SCC) default findings for data exfiltration."_

- SCC se enfoca en la **postura de seguridad de los recursos de GCP** (configuraciones, vulnerabilidades) — no está diseñado específicamente para analizar **patrones de comportamiento de descarga de datos por usuario** dentro de los logs ingeridos en SecOps. No es la herramienta nativa correcta para este caso de uso de comportamiento de usuario.

#### Por qué no B

> _"Create a log-based metric in Cloud Monitoring with a predefined limit, and identify users who exceed it in SecOps."_

- Igual que C, esto usa un **límite predefinido/fijo** — no una línea base individual por usuario. Además, requiere **construir manualmente** la métrica, configurar la alerta, y luego cruzar esos datos de vuelta hacia SecOps — un proceso con **más pasos y más esfuerzo de configuración manual** comparado con simplemente habilitar UEBA, que ya integra esta capacidad de forma nativa dentro de la misma plataforma.

---

#### La idea clave para el examen

Cuando el enunciado hable específicamente de **comparar contra la "línea base establecida" de un usuario/entidad** (no un umbral fijo genérico) y pida la solución de **menor esfuerzo**, la respuesta casi siempre apunta a **UEBA** (User and Endpoint Behavioral Analytics) — porque es la funcionalidad **nativa y curada** de Google SecOps diseñada exactamente para modelar comportamiento individual y detectar desviaciones, evitando que tengas que construir manualmente lógica de detección con umbrales fijos (YARA-L personalizado o métricas de Cloud Monitoring), que además no capturan bien el concepto de "baseline individual."