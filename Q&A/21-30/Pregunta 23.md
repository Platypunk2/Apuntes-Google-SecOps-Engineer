![[Pasted image 20260908182622.png]]

**Qué es el IC-Score (Indicator Confidence Score)**

Es un puntaje que representa **qué tan confiable/fiable es un indicador de compromiso (IOC)** — es decir, qué tan seguro está el proveedor de threat intelligence (en este caso, Mandiant/Google Threat Intelligence) de que ese indicador realmente representa actividad maliciosa, y no solo una coincidencia genérica o de baja calidad.

---

**Qué pide el escenario**

- Estás usando **Applied Threat Intelligence (ATI)** con reglas YARA-L para hacer matching de IOCs

- Hay **más falsos positivos de los esperados**, generando ruido para el SOC

- Necesitas **reducir esos falsos positivos**

---

**Por qué A es correcta**

- El problema de fondo es que las reglas probablemente están haciendo match contra **cualquier IOC**, sin filtrar por qué tan confiable es ese indicador — incluyendo IOCs de **baja confianza** (que podrían ser coincidencias débiles, indicadores viejos/obsoletos, o de baja certeza).

- Al modificar las reglas YARA-L para que **solo generen alerta cuando el IC-Score sea 60% o superior**, estás elevando el **umbral de confianza mínimo** requerido para que un IOC dispare una detección — esto filtra directamente los indicadores de **baja calidad/confianza** que están generando el ruido, sin perder la capacidad de detectar amenazas reales (que normalmente tienen un IC-Score alto).

- Esto se implementa directamente en la sección `outcome`/`condition` de la regla YARA-L, evaluando el campo de confidence score del contexto del IOC — es una solución **precisa y dirigida** al problema exacto descrito (demasiados falsos positivos por indicadores de baja confianza).

---

**Por qué no las demás**

**B** (Configurar agrupación de alertas — alert grouping):

- El **alert grouping** ayuda a **consolidar** alertas repetitivas relacionadas en una sola vista (reduce el volumen visual de alertas), pero **no reduce los falsos positivos en sí** — solo los agrupa. Las alertas erróneas siguen ahí, solo que agrupadas; el problema de fondo (falta de precisión en el matching de IOCs) no se soluciona.

**C** (Implementar detecciones curadas en vez de reglas YARA-L personalizadas):

- Las curated detections son reglas predefinidas por Google, útiles como punto de partida, pero cambiar todo tu enfoque de detección es una solución mucho más drástica y de mayor esfuerzo que simplemente ajustar el umbral de confianza en las reglas que ya tienes.

- Además, no garantiza resolver el problema — las detecciones curadas también pueden generar falsos positivos si no se ajusta el contexto de confianza de los IOCs correspondientes.

**D** (Playbook que auto-ajusta la fuente del IOC si el IC-Score está entre 60% y 80%):

- Esto es una solución de **automatización de tuning de la fuente de IOCs**, no de la **regla de detección en sí**. Es un enfoque más indirecto: en vez de simplemente subir el umbral de confianza requerido en la regla (como en A), intenta "ajustar" dinámicamente la fuente — lo cual añade complejidad innecesaria (un playbook completo) para resolver algo que se soluciona de forma más simple y directa modificando el umbral en la regla misma.

- Es una solución más compleja para el mismo problema, cuando existe una opción más simple y directa (A).

---

**La idea clave para el examen**

Cuando el escenario mencione **"demasiados falsos positivos"** en el contexto de **Applied Threat Intelligence / matching de IOCs**, la solución más directa es **elevar el umbral del IC-Score** en las reglas de detección — filtrando indicadores de baja confianza directamente en la lógica de la regla, en lugar de soluciones indirectas como agrupar alertas, cambiar todo el enfoque de detección, o automatizar el ajuste de la fuente con un playbook.