![[Pasted image 20260908190424.png]]

**Qué pide el escenario**

- Ya tienes una **pista concreta**: actividad repetida de PowerShell + conexiones salientes a un dominio sospechoso, en **múltiples sistemas y cuentas de usuario**

- El dominio **no está en tus feeds de threat intel** → no puedes depender de IOCs conocidos

- Necesitas **correlacionar** actividad a través de **sistemas e identidades** para entender el **alcance completo** del compromiso (qué otros endpoints/usuarios están afectados)

Esto es literalmente la definición de un problema de **correlación multi-evento**: no buscas un solo evento aislado, sino **patrones relacionados que se repiten a través de distintas entidades** (endpoints, usuarios) en una ventana de tiempo.

---

**Por qué A es correcta**

- YARA-L 2.0 es el **lenguaje unificado** de Google SecOps para búsqueda, dashboards y reglas de detección, diseñado específicamente para **correlacionar múltiples eventos** usando su estructura de secciones (`events`, `match`, `outcome`, `condition`).

- Con una **búsqueda de múltiples eventos (multi-event)**, usando la sección `match` con las claves de agrupación correctas (por ejemplo, agrupar por `principal.hostname` o `principal.user.userid`), puedes **agrupar y contar** cuántos sistemas/usuarios distintos muestran el mismo patrón (PowerShell + conexión al dominio sospechoso) dentro de una ventana de tiempo — exactamente lo necesario para **mapear el alcance del compromiso** (scope) e identificar qué usuario podría ser el origen o el más comprometido.

- Esto se hace directamente desde la **búsqueda** (no necesitas convertirlo aún en una regla formal), permitiéndote iterar rápido mientras investigas.

---

**Por qué no las demás**

**B** (Búsqueda de log crudo + pivotear manualmente):

- Es un enfoque **manual y no escalable**: pivotear a mano usuario por usuario, sistema por sistema, es lento e ineficiente cuando ya sabes que la actividad ocurre "across multiple systems and user accounts" — precisamente el escenario donde una **correlación automatizada** (YARA-L) es mucho más eficiente y menos propensa a error humano (podrías pasar por alto una conexión).

**C** (User Sign-In Overview dashboard):

- Este dashboard se enfoca en **tendencias de autenticación** (logins, anomalías de sign-in) — no tiene relación directa con el patrón descrito, que es sobre **actividad de PowerShell y conexiones salientes a un dominio**, no sobre patrones de login. No te ayuda a correlacionar este tipo específico de actividad.

**D** (Behavioral Analytics dashboard en Risk Analytics):

- Este dashboard se centra en **actividad basada en IP anómala y comportamiento de riesgo de usuario** de forma más general/agregada (UEBA) — es útil para detectar anomalías de comportamiento a lo largo del tiempo, pero **no está diseñado para hacer una investigación dirigida y específica** de un patrón ya identificado (PowerShell + dominio específico) a través de sistemas y usuarios concretos.

- Es más una herramienta de **detección de anomalías generales**, mientras que aquí ya tienes un **IOC de comportamiento específico** (el patrón PowerShell + dominio) que necesitas **rastrear directamente** — para eso, una búsqueda YARA-L dirigida es más precisa y directa que un dashboard de comportamiento general.

---

**La idea clave para el examen**

Cuando ya tienes un **patrón de actividad específico identificado** (no solo "algo anómalo en general") y necesitas **correlacionarlo across múltiples sistemas/usuarios** para determinar el alcance de un compromiso, la respuesta apunta a una **búsqueda/regla multi-evento en YARA-L 2.0** — es la herramienta de correlación nativa y precisa de SecOps, a diferencia de dashboards de comportamiento general (Behavioral Analytics, Sign-In Overview) que sirven para **detectar** anomalías amplias, no para **investigar y correlacionar** un patrón ya conocido de forma dirigida.