![[Pasted image 20260908232635.png]]

#### Qué pide el escenario

- La regla detecta **"conexiones de red excesivas"** pero **dispara con demasiada frecuencia** (falsos positivos)
- Necesitas **reducir el ruido** sin **perder efectividad** (es decir, sin dejar de detectar comportamiento genuinamente sospechoso)

---

#### Por qué A es correcta

> _"Add a threshold in the YARA-L condition: section to ensure that the rule only alerts after a certain number of connections."_

- El problema real es que la regla probablemente está disparando con **cualquier cantidad de conexiones que cumplan el patrón** (por ejemplo, con solo 1 o 2 conexiones excesivas ya genera una alerta), sin un **umbral mínimo significativo**.
- La sección **`condition`** en YARA-L es exactamente donde defines la **lógica final que debe cumplirse** para disparar la regla — por ejemplo, algo como `$connection_count > 100` en lugar de solo `$connection_count > 0`.
- Al agregar un **threshold numérico apropiado** (basado en lo que realmente representa un volumen "anómalo" según tu contexto normal de tráfico), filtras el ruido de conexiones que técnicamente cumplen el patrón pero **no son suficientemente anómalas** como para justificar una alerta — sin perder la capacidad de detectar los casos **genuinamente excesivos**, que seguirán superando el umbral.
- Es la solución que ataca **directamente la causa raíz del ruido**: la falta de un criterio de "cuánto es demasiado" bien calibrado.

---

#### Por qué no C (la opción más parecida técnicamente)

> _"Include a 10 minute timeframe for the same source and destination in the YARA-L match: section to aggregate the alerts."_

- La sección **`match`** con una ventana de tiempo sirve para **agrupar/correlacionar eventos relacionados** dentro de un periodo — es decir, consolida múltiples eventos individuales en **una sola detección**, reduciendo el **volumen de alertas duplicadas** sobre el mismo patrón.
- El problema es que esto **no resuelve la causa raíz**: sigue sin haber un criterio de "cuántas conexiones son demasiadas" — solo estás **agrupando** las alertas repetidas en menos notificaciones, pero la regla **seguiría disparando** con la misma sensibilidad (baja) que antes, solo que ahora consolidada en un paquete de 10 minutos. Sigue siendo ruidosa en términos de **qué tan fácil es que se dispare**, solo cambia la frecuencia de notificación, no la precisión del criterio de detección.
- Es un cambio de **presentación/agregación**, no de **precisión de detección**.

#### Por qué no B

> _"Assign a risk score in the YARA-L outcome: section to prioritize alerts more effectively."_

- Asignar un risk score ayuda a **priorizar** qué alertas revisar primero, pero **no reduce el número de alertas generadas** — la regla seguiría disparando con la misma frecuencia; solo tendrías una forma de ordenarlas por importancia. No ataca el problema de "demasiadas alertas", solo ayuda a **navegar** ese volumen.

#### Por qué no D

> _"Update the YARA-L events: section to exclude the most common IP addresses..."_

- Excluir las IPs más comunes es una solución **frágil y potencialmente peligrosa**: podrías estar **excluyendo exactamente las IPs que un atacante decide usar** si sabe (o adivina) que son parte de una lista de exclusión, o podrías **perder detección legítima** si una IP "común" en algún momento se vuelve maliciosa (por ejemplo, un servidor interno comprometido). Reduce el ruido de forma **poco robusta y sin criterio de comportamiento real** — contradice el requisito de "sin reducir la efectividad de la regla."

---
#### La distinción clave para el examen

- **`condition` con threshold** → ataca la **precisión/sensibilidad** de la detección (cuántas ocurrencias son necesarias para considerar algo "excesivo") — esto es lo que se necesita cuando el problema es que la regla dispara **con muy poca evidencia**.
- **`match` con ventana de tiempo** → ataca la **consolidación/agregación** de eventos relacionados (agrupar detecciones repetidas en una sola alerta) — útil cuando el problema es **ruido por duplicación**, no por baja sensibilidad del criterio de disparo.

Cuando el enunciado diga que la regla **"triggers too frequently"** sin más contexto sobre duplicación, la solución más directa y efectiva es **ajustar el threshold en la condición**, porque ataca la raíz del problema: la definición de qué constituye "excesivo."