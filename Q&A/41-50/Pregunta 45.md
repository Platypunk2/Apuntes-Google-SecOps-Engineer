![[Pasted image 20260908233047.png]]
#### Qué pide el escenario

- La service account actualmente tiene **solo permisos de lectura** (read) sobre findings de SCC a nivel de organización
- Puede **leer** los findings correctamente, pero falla al intentar **actualizar el estado** de los findings (write/update)
- Necesitas resolver el error de permisos siguiendo el **principio de mínimo privilegio** (least privilege)
---

#### Por qué B es correcta

> _"Grant the service account the roles/securitycenter.findingsEditor IAM role at the organization level."_

- El síntoma es claro: **lectura funciona, escritura falla** → es un problema de **permisos insuficientes para la acción específica de actualizar/modificar** findings.
- El rol **`roles/securitycenter.findingsEditor`** es el rol de IAM diseñado exactamente para otorgar permisos de **edición/actualización de findings** en SCC (cambiar su estado, marcarlos como resueltos, etc.) — es el complemento natural del rol de solo lectura que ya tiene la cuenta.
- Otorgarlo **a nivel de organización** es necesario porque la integración actual ya opera con alcance de **organización completa** (la cuenta ya lee findings a ese nivel) — para mantener consistencia funcional, el permiso de edición debe aplicarse al **mismo alcance** donde ya se están leyendo y gestionando los findings.
- Es la solución **mínima y específica**: agrega exactamente el permiso que falta, sin over-provisioning, cumpliendo el principio de **mínimo privilegio**.

---

#### Por qué no las demás

**A** (`roles/securitycenter.findingsBulkMuteEditor`):

- Este rol es específico para **operaciones de "bulk mute"** (silenciar findings de forma masiva) — es un permiso **mucho más limitado y específico** que no cubre la actualización general de estados de findings que describe el enunciado. No es el rol correcto para la funcionalidad de "update finding states" en términos generales.

**C** (`roles/iam.serviceAccountUser` a sí misma):

- Como vimos en preguntas anteriores (pregunta 20), este rol permite **actuar como/impersonar** una cuenta de servicio — no tiene relación con permisos de **lectura o escritura sobre findings de SCC**. No resuelve el problema de permisos descrito.

**D** (Regenerar la clave del service account y actualizar credenciales):

- Esto sería la solución correcta **si el problema fuera de autenticación** (credenciales inválidas, expiradas, o rotación de claves) — pero el enunciado deja claro que la **autenticación funciona bien** (la cuenta **sí puede leer** datos exitosamente). El error es específicamente de **autorización** para una acción particular (escritura), no de identidad/autenticación. Regenerar la clave no añade ningún permiso nuevo — el problema seguiría existiendo después de este cambio.

---

#### La idea clave para el examen

Cuando el síntoma sea **"puede leer pero no puede escribir/actualizar"**, el diagnóstico es casi siempre un problema de **permisos IAM insuficientes para la acción de escritura específica** — necesitas identificar y otorgar el **rol predefinido correcto** que cubre esa acción (en este caso, `findingsEditor` para actualizar el estado de findings), evitando roles demasiado específicos/limitados (como `findingsBulkMuteEditor`) o soluciones que atacan el problema equivocado (como regenerar credenciales, que resuelve fallos de **autenticación**, no de **autorización**).