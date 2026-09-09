![[Pasted image 20260908233612.png]]

#### Qué pide el escenario

- Escribir una regla de detección en **SIEM (SecOps)** que envíe un **risk score** a la alerta
- Ya tienes **acceso a datos de GTI** vía tu suscripción de SecOps (es decir, **el enriquecimiento ya está disponible**, no necesitas configurarlo desde cero)
- El **threat score de GTI** debe **informar directamente** el risk score de la alerta
- Ese resultado debe quedar **disponible para futuras detecciones**

---

#### Por qué A es correcta

> _"Use the outcomes section of your detection logic to pull UDM enrichment fields from the event data. Apply logic to determine the total risk outcome, and store the risk score as the risk_score variable."_

- Como ya tienes **GTI integrado a través de tu suscripción de SecOps**, los datos de threat intelligence de Google **ya llegan como campos de enriquecimiento dentro del UDM** (contexto asociado a las entidades del evento) — no necesitas construir ninguna integración nueva.
- La sección **`outcome`** de una regla YARA-L es exactamente donde se **extraen esos campos de contexto/enriquecimiento** (por ejemplo, el threat score de GTI ya presente en el evento enriquecido), se les **aplica lógica** (por ejemplo, ponderarlos, combinarlos con otros factores), y se **calcula el resultado final** que se asigna a la variable especial **`risk_score`**.
- La variable `risk_score` en YARA-L es un **outcome reservado y reconocido por la plataforma**: cuando la asignas, ese valor se convierte directamente en el **risk score de la alerta/detección generada**, y queda **almacenado como parte del resultado de la detección** — disponible para futuras referencias, correlaciones o reglas compuestas.
- Es la solución **más directa y nativa**: usa datos que **ya están disponibles** (GTI ya viene incluido) y el mecanismo **correcto y reservado** de YARA-L para asignar el risk score de forma que sea reconocido y persistido por la plataforma.

---

#### Por qué no las demás

**B** (Usar la sección `match` para filtrar entidades y "almacenar las restantes" como `risk_score`):

- Esto es **conceptualmente incorrecto**: la sección `match` sirve para **agrupar/correlacionar eventos** por claves comunes (usuarios, IPs, etc.) dentro de una ventana de tiempo — **no es donde se calcula ni se asigna el risk score**. Además, "almacenar las entidades restantes" como risk_score no tiene sentido semántico: risk_score debe ser un **valor numérico**, no una lista de entidades filtradas.

**C** (Configurar un feed para ingerir datos de GTI):

- Esto es **innecesario y redundante**: el enunciado dice explícitamente que **ya tienes acceso a datos de GTI a través de tu suscripción de SecOps** — es decir, el enriquecimiento **ya está disponible de forma nativa**, sin necesitar configurar un feed adicional. Configurar un feed manual sería un paso extra e inútil, dado que este enriquecimiento ya viene integrado con la suscripción.

**D** (Crear un playbook SOAR que consulte GTI vía integración de VirusTotal, y modificar el risk_score vía context):

- Este enfoque introduce una **capa de complejidad innecesaria**: usar SOAR (orquestación/automatización) para una tarea que debería resolverse directamente **dentro de la lógica de detección de SIEM** (la regla YARA-L en sí). El enunciado pide específicamente una solución **dentro de la regla de detección**, no un playbook externo que corra después de generada la alerta.
- Además, modificar el "risk_score context value" desde SOAR es un enfoque más indirecto y desacoplado, cuando ya existe el mecanismo nativo de `outcome`/`risk_score` directamente en YARA-L (como en A) para lograrlo de forma más simple y directa dentro del propio flujo de detección.

---

#### La idea clave para el examen

Cuando necesites que el **risk score de una alerta** se calcule usando **datos de threat intelligence ya integrados** (como GTI vía suscripción de SecOps) dentro de una **regla de detección YARA-L**, la solución nativa es usar la sección **`outcome`** para extraer esos campos de enriquecimiento UDM, aplicar la lógica de cálculo, y asignar el resultado a la variable reservada **`risk_score`** — evitando confundir esto con la sección `match` (que es para correlación/agrupación), o con soluciones innecesariamente complejas que involucren configurar feeds redundantes o desviar la lógica hacia playbooks de SOAR cuando el cálculo pertenece naturalmente a la capa de detección del SIEM.