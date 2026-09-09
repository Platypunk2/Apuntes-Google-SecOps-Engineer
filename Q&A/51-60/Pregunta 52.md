![[Pasted image 20260909000311.png]]

#### Qué pide el escenario

- Necesitas **monitoreo y alertas** para VMs que cumplan **DOS condiciones simultáneas**:
    1. Tienen el tag/label **`compliance=pci`**
    2. Tienen una **IP externa asignada**

---

#### Por qué D es correcta

> _"Use the PUBLIC_IP_ADDRESS Security Health Analytics (SHA) detector to identify Compute Engine instances with external IP addresses. Determine whether the compliance=pci tag exists on the instances."_

- **SHA (Security Health Analytics)** ya tiene un **detector nativo predefinido** llamado exactamente **`PUBLIC_IP_ADDRESS`**, que **automáticamente** genera un finding cada vez que detecta una VM con IP externa asignada — **sin que tengas que construir nada desde cero**.
- Como cada finding de SCC **incluye los metadatos del recurso afectado** (incluyendo sus **labels/tags**, como `compliance=pci`), puedes **filtrar/correlacionar** directamente sobre esos findings ya generados para identificar cuáles corresponden específicamente a instancias con ese tag — usando las capacidades **nativas de filtrado** de la consola de SCC (por label/tag del recurso), sin necesitar programación adicional.
- Es la solución de **mínimo esfuerzo**: reutilizas un detector **ya construido y mantenido por Google**, y simplemente **filtras** sus resultados según el criterio adicional (`compliance=pci`) que necesitas — en lugar de construir toda la lógica de detección desde cero.

---
#### Por qué no C (la opción más "parecida" y tentadora)

> _"Create a custom Security Health Analytics (SHA) module. Configure the detection logic to scan Cloud Asset Inventory data for compute.googleapis.com/Instance assets, and search for the compliance=pci tag."_

- Esto implica **construir un módulo de detección personalizado desde cero**: definir manualmente la lógica de consulta contra Cloud Asset Inventory, buscar el tag específico, y **presumiblemente también** tendrías que agregar la lógica para verificar la condición de IP externa (ya que el enunciado describe la condición compuesta: tag **Y** IP externa).
- Esto representa **mucho más esfuerzo de desarrollo y mantenimiento** que simplemente **aprovechar el detector `PUBLIC_IP_ADDRESS` ya existente** (que ya cubre la mitad de la condición de forma nativa) y filtrar sus resultados por el tag — es "reinventar la rueda" cuando ya existe una pieza clave lista para usar.

#### Por qué no A

> _"Create a custom Event Threat Detection module that alerts when a Compute Engine instance with the compliance=pci tag is assigned an external IP address."_

- **Event Threat Detection (ETD)** está diseñado para detectar **actividad de amenazas basada en eventos de logs** (comportamiento sospechoso, indicadores de compromiso) — no es la herramienta adecuada para monitorear **configuraciones estáticas de recursos** (como si una VM tiene o no una IP pública asignada). Ese tipo de verificación de **postura/configuración** es exactamente el dominio de **Security Health Analytics**, no de ETD.

#### Por qué no B

> _"Deploy the compute.vmExternalIpAccess organization policy constraint to prevent... from creating Compute Engine instances with external IP addresses."_

- Esta opción es una medida de **prevención/governance a futuro** (evita que se **creen** nuevas VMs con IP externa en proyectos con ese tag) — pero el enunciado pide específicamente **"monitoreo y alertas"**, no bloqueo preventivo. Además, no ayuda a **detectar instancias que ya existen actualmente** con esa configuración, ni genera ningún tipo de alerta/finding — es una herramienta diferente para un objetivo diferente (prevención vs. detección/monitoreo).

---

#### La idea clave para el examen

Cuando el escenario necesite **monitoreo/alertas sobre una condición de configuración de recursos** (como IPs públicas) que además debe **cruzarse con un tag/label específico**, la solución de **menor esfuerzo** suele ser: **usar el detector nativo de SHA que ya cubre la condición principal** (en este caso `PUBLIC_IP_ADDRESS`) y **filtrar/correlacionar sus resultados** por el criterio adicional (el tag), en lugar de construir un módulo de detección completamente personalizado desde cero cuando gran parte de la lógica ya existe de forma nativa en la plataforma.
