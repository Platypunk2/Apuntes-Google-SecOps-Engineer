![[Pasted image 20260908210023.png]]

#### Qué pide el escenario

- Identificar **nodos C2 desconocidos** (no conocidos previamente) que podrían estar activos
- Necesitas un **resultado accionable (lista de posibles matches) en 24 horas** — es decir, hay una restricción de **tiempo/plazo concreto**
- Debe cubrir **tráfico histórico** (para detectar si ya hubo comunicación con C2 en el pasado reciente, no solo desde ahora en adelante)

---

#### Por qué A es correcta

> _"Write a rule in Google SecOps that scans historic network outbound connections against ingested threat intelligence. Run the rule in a retrohunt against the full tenant."_

- El **retrohunt** en Google SecOps está diseñado exactamente para esto: tomar una regla de detección y **aplicarla retroactivamente contra datos históricos** ya ingeridos, en lugar de esperar a que ocurran eventos nuevos en tiempo real.
- Al comparar las conexiones salientes históricas contra **threat intelligence ya ingerida** (IOCs de C2 conocidos), obtienes rápidamente una lista de **coincidencias existentes** — esto es preciso, dirigido, y aprovecha infraestructura **ya nativa de SecOps**, lo cual permite generar resultados **dentro del plazo de 24 horas** sin necesidad de construir pipelines nuevos.
- Es la solución de **menor esfuerzo y mayor velocidad** entre las opciones, usando exactamente las herramientas que la plataforma ya provee para este propósito.

---

#### Por qué no las demás

**B** (Cargar registros de red en BigQuery para detectar comunicación fuera de 3 desviaciones estándar):

- Esto requiere **construir desde cero** un análisis estadístico personalizado: extraer los datos, cargarlos en BigQuery, definir el modelo de "normalidad" y calcular desviaciones estándar. Es un proceso de **ingeniería de datos considerable**, poco realista de completar con precisión en **24 horas**.
- Además, la detección por desviación estándar genera **muchos falsos positivos** (cualquier anomalía de tráfico, no necesariamente C2) y no está anclada a **threat intelligence concreta**, lo que la hace menos precisa que comparar contra IOCs conocidos.

**C** (Revisar findings de Security Health Analytics en SCC):

- **SHA** se enfoca en detectar **malas configuraciones de recursos de GCP** (buckets públicos, IAM excesivo, VMs sin parchear, etc.) — no está diseñado para analizar **tráfico de red saliente** en busca de comunicación con C2. Es la herramienta equivocada para este objetivo específico.

**D** (Regla YARA-L que compara tráfico contra dominios de baja prevalencia + registros WHOIS recientes):

- Esta es una **heurística de detección genuina** (dominios recién registrados + baja prevalencia son señales típicas de infraestructura C2), pero tiene un problema práctico: requeriría **datos de WHOIS integrados/enriquecidos** que probablemente no estén ya disponibles/ingeridos en tu entorno, y construir esa lógica de correlación desde cero en 24 horas es más complejo y menos directo que aprovechar **threat intelligence ya existente** (opción A).
- Es una técnica más avanzada y con más **falsos positivos potenciales** (dominios nuevos y de baja prevalencia no son automáticamente maliciosos), comparado con hacer matching directo contra indicadores **ya confirmados** como maliciosos.

---

#### La idea clave para el examen

Cuando el escenario combine **"generar resultados en un plazo corto (24 horas)"** + **"analizar datos históricos"** + **"amenaza conocida por tipo pero no por indicador específico (C2 desconocidos)"**, la solución más eficiente es usar el **retrohunt de Google SecOps** con una regla que compare contra **threat intelligence ya ingerida** — aprovechando infraestructura nativa ya lista, en lugar de construir análisis estadísticos personalizados (BigQuery) o heurísticas más complejas (WHOIS + prevalencia) que requieren más tiempo de desarrollo y validación.