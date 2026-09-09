![[Pasted image 20260909091151.png]]

#### Qué pide el escenario

- **Identificar todos los IOCs potenciales** de Google Threat Intelligence que ya están correlacionados contra los datos de tu organización
- No dice "crear una nueva capacidad de detección" — dice **identificar lo que ya existe/se ha correlacionado**

---

#### Por qué D es correcta

> _"Use the Alerts & IOCs page in Google SecOps."_

- La página **"Alerts & IOCs"** es la vista **centralizada y nativa** dentro de Google SecOps diseñada exactamente para este propósito: mostrar **todas las coincidencias de IOCs** (Indicators of Compromise) que Google Threat Intelligence/Applied Threat Intelligence ha **correlacionado automáticamente** contra tus datos UDM ingeridos.
- Es la vista de **menor esfuerzo y más directa**: no necesitas escribir ninguna regla, ni construir ninguna búsqueda personalizada — simplemente **navegas a esa página** y ves de forma consolidada todos los matches de IOCs relevantes para tu organización.

---

#### Por qué no las demás

**A** (Usar la página de Cases):

- La página de **Cases** muestra los **casos de investigación** ya creados (agrupaciones de alertas que un analista está trabajando) — no es una vista diseñada para **identificar** IOCs directamente; es una capa posterior de gestión de incidentes, no de descubrimiento de indicadores.

**B** (Crear reglas YARA-L para detectar y alertar cuando GTI identifique amenazas):

- Esto es una solución de **mucho mayor esfuerzo**: requiere **desarrollar y mantener** lógica de detección personalizada, cuando **ya existe una funcionalidad nativa** (la página Alerts & IOCs) que hace exactamente esto de forma automática, sin necesidad de escribir ninguna regla. Es "reinventar la rueda."

**C** (Usar Gemini para buscar amenazas potenciales contra los datos de la organización):

- Gemini es útil para **generar contenido, resumir, o ayudar a construir reglas/consultas** — pero no es la herramienta diseñada específicamente para **mostrar la correlación de IOCs ya identificados** por Google Threat Intelligence. No es la vía nativa y directa para este objetivo específico.

---

#### La idea clave para el examen

Cuando el objetivo sea **"identificar" IOCs de threat intelligence ya correlacionados** contra tus datos (no crear nueva lógica de detección desde cero), la respuesta apunta a la **vista nativa diseñada para eso** — en este caso, la página **"Alerts & IOCs"** de Google SecOps — en lugar de construir reglas personalizadas, usar herramientas de IA generales, o navegar a vistas de gestión de casos que no están diseñadas para el descubrimiento de indicadores.