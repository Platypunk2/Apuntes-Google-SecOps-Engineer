![[Pasted image 20260909001259.png]]

### Qué pide el escenario

Recibiste un IOC (Indicador de Compromiso) — un dominio sospechoso usado para C2 (Command and Control) — desde tu feed de threat intelligence. Necesitas investigar en **Google SecOps (Chronicle)** si ese dominio apareció en tu entorno, y quieres la forma **más eficiente** de buscarlo.

La palabra clave es "más eficiente": no basta con encontrar una forma que funcione, se busca la búsqueda estructurada, dirigida y rápida — no un método lento o pasivo.

---

### Por qué B es correcta: UDM search en la sección DNS

**UDM (Unified Data Model)** es el modelo de datos normalizado de Google SecOps. Todos los logs ingeridos (firewall, DNS, proxy, endpoint, etc.) se parsean y normalizan a este esquema común, sin importar el fabricante original del log.

Un dominio C2 típicamente se resuelve mediante consultas DNS antes de que ocurra la comunicación C2 real. Por eso:

- Una **búsqueda UDM dirigida al campo DNS** (`network.dns.questions.name`, por ejemplo) es precisa y rápida: buscas directamente el campo estructurado donde aparecería ese dominio.
- Es eficiente porque usa el esquema normalizado, indexado y optimizado, en lugar de examinar texto sin procesar.
- Te permite correlacionar directamente ese dominio contra eventos DNS reales en tu entorno.

---

### Por qué las otras opciones NO son la mejor opción

**A. Raw log search (búsqueda de logs sin procesar)**

- Busca el string del dominio en logs crudos, sin usar el esquema normalizado.
- Es **mucho menos eficiente**: no aprovecha la indexación UDM, es más lento, y puede generar más ruido/falsos positivos porque no está buscando en un campo específico y estructurado.

**C. Group by Field en scan view (agrupar por hostname)**

- Esto sirve para **agrupar y visualizar patrones** entre eventos (ej. ver qué hostnames son más frecuentes), no para buscar un IOC específico.
- No es una técnica de búsqueda dirigida a un IOC conocido; es más una herramienta de análisis exploratorio/clustering, no de investigación puntual.

**D. IOC Search feature y esperar detecciones en Case view**

- El IOC Search / IOC matching en SecOps generalmente funciona de forma **pasiva**: coincide automáticamente los IOCs de tus feeds de threat intelligence contra el tráfico **según va llegando** (en el futuro), no necesariamente contra los datos históricos ya ingeridos.
- "Esperar" a que aparezcan detecciones no es una investigación activa ni eficiente — es un enfoque reactivo/pasivo, no una búsqueda inmediata de si el dominio **ya apareció** en el entorno.