![[Pasted image 20260909002045.png]]

El equipo de riesgo y cumplimiento necesita **reportes regulares y recurrentes** de cumplimiento contra **frameworks de control estándar de la industria** (ej. CIS, NIST, PCI-DSS, etc.) para una unidad de negocio regulada. Además, el entorno **continuamente agrega proyectos nuevos**, y el reporte debe incluir **evidencia de recursos no conformes**.

Elementos clave a resolver:

1. Cumplimiento contra **frameworks estándar de la industria** (no reglas personalizadas).
2. **Automatización/recurrencia** — se agregan proyectos continuamente, así que la solución debe escalar y adaptarse sola.
3. **Evidencia concreta** de recursos no conformes, no solo un puntaje.
4. Mínimo esfuerzo de mantenimiento (no quieres reconstruir reglas manualmente cada vez que llega un proyecto nuevo).

---

### Por qué D es correcta: Posture Management de SCC con framework integrado

**Security Command Center (SCC) Posture Management** incluye **frameworks de cumplimiento pre-construidos** (built-in) para estándares reconocidos de la industria (CIS Benchmarks, NIST, PCI-DSS, ISO 27001, etc.).

- Al usar el **posture built-in para el framework de compliance requerido**, obtienes automáticamente las reglas/controles ya mapeados a ese estándar — sin necesidad de programarlos manualmente.
- SCC Posture **monitorea continuamente** todos los recursos del entorno, incluyendo proyectos que se agregan después — se adapta automáticamente al crecimiento del entorno regulado.
- Genera **evidencia específica de los recursos no conformes** (findings), que es justo lo que pide el escenario.
- Es una solución **nativa, gestionada y de bajo mantenimiento**, ideal para reportes recurrentes sin trabajo manual constante.

---

### Por qué las otras opciones NO son la mejor opción

**A. Audit Manager**

- Audit Manager en Google Cloud está más orientado a **gestionar el proceso de auditoría** (evidencia recopilada, flujos de trabajo de auditoría, coordinación con auditores) que a la **evaluación técnica continua y automática** de cumplimiento contra un framework de control mientras el entorno crece.
- No está diseñado para escalar automáticamente conforme se agregan proyectos nuevos de forma nativa como SCC Posture.

**B. Consultas en BigQuery sobre Cloud Asset Inventory**

- Esto requiere **construir y mantener manualmente** las consultas que representen cada control del framework — trabajo significativo de ingeniería.
- Cuando se agregan proyectos nuevos continuamente, tendrías que asegurarte de que las consultas cubran automáticamente los nuevos recursos, lo cual es más frágil y requiere mantenimiento constante.
- No aprovecha un framework de compliance ya mapeado y mantenido por Google.

**C. Rego + Workload Manager**

- Esto implica **construir el framework de control desde cero en Rego** (lenguaje de políticas), lo cual es un esfuerzo considerable de desarrollo y mantenimiento.
- Workload Manager está más enfocado en cargas de trabajo específicas (ej. SAP, bases de datos) y validación de mejores prácticas de configuración, no es la herramienta principal para compliance reporting a nivel de framework de industria en toda la organización.
- Reinventar un framework estándar en lugar de usar uno ya integrado es ineficiente.