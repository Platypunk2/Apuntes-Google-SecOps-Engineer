![[Pasted image 20260908195101.png]]

Aquí está el matiz clave que distingue esta pregunta, y por qué la respuesta correcta es **A** y no **C** (que a primera vista parece la más obvia, dado que "Collections" en Google Threat Intelligence está literalmente diseñado para compartir IOCs).

#### La frase clave del enunciado

> _"...share the IOCs with other teams for collaboration **and integration into their operational processes**."_

Esto no es solo "compartir para que otros vean/colaboren en investigación" — es **compartir para que otros equipos puedan usar operacionalmente esos IOCs** dentro de sus propios procesos (por ejemplo, en sus reglas de detección, playbooks, listas de bloqueo, etc.).

---

#### Por qué A es correcta

> _"Create a list in Google Security Operations (SecOps), and grant the required access to the other teams."_

- Las **Lists (listas de referencia)** en Google SecOps son objetos diseñados específicamente para ser **consumidos directamente por el motor de detección**: se pueden referenciar dentro de reglas YARA-L, playbooks de SOAR, y otros procesos operativos de seguridad.
- Al crear una lista con los IOCs y otorgar acceso a otros equipos, esos equipos pueden **integrar inmediatamente** esa lista en **sus propias reglas de detección y flujos de trabajo operativos** dentro de la misma plataforma SecOps — cumpliendo exactamente el requisito de **"integration into operational processes"**.
- Es la ruta más **directa y nativa** dentro del propio ecosistema operativo de SecOps, sin pasos adicionales de exportación/importación.

---

#### Por qué no C (el "distractor" más convincente)

> _"Add the IOCs to a collection in Google Threat Intelligence, and share the collection with the other teams."_

- Las **Collections** en Google Threat Intelligence son excelentes para **colaboración de investigación e inteligencia de amenazas**: agregan contexto, análisis, telemetría, y permiten compartir hallazgos con otros analistas o equipos (incluso fuera de tu organización).
- Sin embargo, su propósito principal es **colaboración analítica/investigativa** (entender la amenaza, ver contexto enriquecido, TTPs asociados), **no** ser el mecanismo directo de **integración operativa** dentro de procesos de detección de otros equipos. Compartir una collection le da a otro equipo _visibilidad_ sobre los IOCs, pero no los deja **listos para usarse directamente** en sus reglas o playbooks de forma nativa como sí lo permite una Lista de SecOps.

#### Por qué no B

> _"Export the IOCs in CSV/JSON, and email the file to the other teams."_

- Es el enfoque más **manual e ineficiente**: requiere que cada equipo **importe manualmente** el archivo a su propio sistema, sin ninguna sincronización ni actualización automática. Contradice el requisito de hacerlo de forma **"rápida y eficiente"**.

#### Por qué no D

> _"Create a new threat graph, and share the graph with the other teams."_

- Un **threat graph** es una herramienta de **visualización de relaciones** entre indicadores (para explorar conexiones entre IOCs, infraestructura, actores) — es útil para **análisis visual**, no es un mecanismo diseñado para **compartir una lista operativa de IOCs** que otros equipos puedan integrar directamente en sus procesos.

---

#### La distinción clave para el examen

- **Collections (GTI)** → colaboración e investigación de threat intelligence entre analistas (contexto, análisis, research)
- **Lists (SecOps)** → integración operativa directa dentro del motor de detección y flujos de trabajo (reglas, playbooks)

Cuando el enunciado enfatice **"integration into operational processes"** (no solo "colaboración"), la señal apunta hacia las **Lists de SecOps**, porque es el objeto diseñado para ser **consumido activamente** por los sistemas de detección — mientras que Collections es la herramienta correcta cuando el objetivo es puramente **compartir contexto de inteligencia** para análisis conjunto.