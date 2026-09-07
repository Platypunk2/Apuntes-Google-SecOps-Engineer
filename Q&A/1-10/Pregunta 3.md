![[Pasted image 20260906180928.png]]

La respuesta correcta a esta pregunta es la **D**.

Google SecOps SOAR tiene una configuración específica en **Settings → Environment Networks** (o "Network Config" según la versión) donde defines los rangos CIDR internos de tu organización. Una vez configurados, la plataforma automáticamente marca (tag) las entidades IP entrantes como "internal" o "external" según coincidan o no con esos rangos, durante el proceso de ingesta del caso.

Por qué las demás no son la mejor opción:

- **A** – Hacer ping desde el Remote Agent es poco fiable (firewalls, ICMP bloqueado) y no es un mecanismo soportado ni escalable para esta clasificación.
- **B** – Ingerir datos de enriquecimiento vía feed es útil para contexto adicional, pero no es el mecanismo nativo diseñado específicamente para clasificar IPs como internas/externas.
- **C** – Modificar la lógica del conector para hacer lookup contra el CMDB añade complejidad innecesaria quando ya existe una función nativa (Environment Networks) diseñada exactamente para este propósito.
