![[Pasted image 20260908225534.png]]

#### Qué pide el escenario

- Ya existe un **finding específico**: una VM dentro del alcance de PCI DSS (CDE) tiene una **IP externa asignada** — esto es una violación de compliance ya detectada
- Necesitas tomar **acción inmediata** para **remediar ese drift específico** ya identificado

---

#### Por qué B es correcta

> _"Reconfigure the network interface settings for the VM to explicitly remove the assigned external IP address."_

- El problema concreto y **ya existente** es que **esa VM específica tiene una IP externa asignada ahora mismo**. La acción de **remediación directa e inmediata** es **quitar esa IP externa** de la configuración de red de la VM.
- Esto ataca el **síntoma actual reportado por el finding**: resuelve la violación de compliance **ya detectada**, de forma directa y específica sobre el recurso afectado.

---

#### Por qué no A (la opción más "tentadora" pero incorrecta para este contexto)

> _"Enable and enforce the constraints/compute.vmExternalIpAccess organization policy constraint at the project level."_

- Esta opción es una medida de **prevención a futuro** (governance): evita que **nuevas VMs** en ese proyecto puedan configurarse con IP externa de ahí en adelante.
- **Pero no resuelve el problema actual**: si la VM **ya tiene** la IP externa asignada, aplicar esta organization policy constraint **no necesariamente la remueve retroactivamente** de una VM ya existente (dependiendo de cómo se aplique, podría requerir que la VM se recree, o simplemente prevenir cambios futuros sin afectar el estado actual).
- Es una excelente práctica de **"cerrar la puerta para que no vuelva a pasar"**, pero el enunciado pide específicamente **remediar el drift ya identificado en esta VM concreta**, de forma **inmediata** — para eso necesitas actuar directamente sobre el recurso (opción B), no solo prevenir configuraciones futuras.

#### Por qué no C

> _"Remove the CDE-specific tag from the VM to exclude it from this particular PCI DSS posture evaluation scan."_

- Esto es una manipulación **completamente incorrecta y peligrosa**: quitar el tag no soluciona el problema de seguridad/compliance real — **solo oculta la VM de la evaluación** de PCI DSS, dejando la vulnerabilidad de configuración (la IP externa) **intacta**. Es literalmente "esconder el problema" en vez de arreglarlo, lo cual podría considerarse una violación aún más grave del espíritu de compliance.

#### Por qué no D

> _"Navigate to the underlying SHA finding for PUBLIC_IP_ADDRESS, and mark this finding as fixed."_

- Marcar un finding como "fixed" (resuelto) manualmente **es solo una actualización de estado/metadato** en el sistema de gestión de findings — **no cambia nada en la configuración real** de la VM. La IP externa **seguiría existiendo** en la infraestructura real; solo estarías falseando el registro de que el problema fue atendido, sin haber hecho la remediación técnica real.

---

#### La idea clave para el examen

Cuando un finding de compliance/postura de seguridad ya identificó una **configuración incorrecta específica en un recurso existente**, y se pide **remediación inmediata**, la respuesta correcta es **corregir directamente la configuración del recurso afectado** (opción B) — no aplicar controles preventivos a nivel de política para el futuro (A, que es un paso complementario recomendado pero no la remediación inmediata en sí), y mucho menos ocultar el hallazgo (C) o falsificar su estado (D), que no resuelven el riesgo de seguridad real subyacente.