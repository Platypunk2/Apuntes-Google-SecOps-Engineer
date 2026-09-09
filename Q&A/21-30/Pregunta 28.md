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

