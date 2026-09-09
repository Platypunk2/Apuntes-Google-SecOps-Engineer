![[Pasted image 20260908233926.png]]

#### Qué pide el escenario

El equipo necesita acceso de **solo lectura** (least privilege) a **tres tipos específicos** de audit logs:

1. **Admin Activity logs**
2. **Data Access logs**
3. **Access Transparency logs**

---
#### Por qué A es correcta

> _"roles/logging.privateLogViewer"_

- Según la documentación oficial: el rol **Logs Viewer (`roles/logging.viewer`)** permite acceder a **todos los logs** almacenados en los buckets `_Required` y `_Default`, **excepto los Data Access logs**.
- El rol **Private Logs Viewer (`roles/logging.privateLogViewer`)** **incluye todos los permisos de `roles/logging.viewer`**, **más** la capacidad adicional de leer los **Data Access audit logs** en el bucket `_Default`.
- Como el enunciado exige explícitamente acceso a **Data Access logs** (además de Admin Activity y Access Transparency), necesitas el rol que **específicamente habilita esa categoría adicional** — que es exactamente `privateLogViewer`.
- Es también la opción que mejor cumple **mínimo privilegio**: es un rol de **solo lectura** (view-only), sin ningún permiso de administración/escritura sobre la configuración de logging.

---

#### Por qué no las demás

**B** (`roles/logging.admin`):

- Este rol otorga **todos los permisos necesarios para usar todas las funciones de Cloud Logging** — incluyendo crear, modificar y eliminar buckets de log, configurar sinks, etc. Esto es **acceso de administración completo**, muy por encima de lo necesario para un equipo que solo necesita **ver** logs. Viola directamente el requisito de **mínimo privilegio**.

**C** (`roles/viewer`):

- Este es un **rol básico genérico** de Google Cloud que otorga acceso de **solo lectura a nivel de todo el proyecto** (no solo logs) — es decir, acceso de lectura a prácticamente **todos los recursos** del proyecto (Compute Engine, Storage, redes, etc.), no específicamente diseñado ni limitado a **logs de auditoría**. Es demasiado amplio e imprecisó para este caso de uso específico, y tampoco garantiza acceso a Data Access logs de forma específica.

**D** (`roles/logging.viewer`):

- Este rol da acceso de lectura a los logs en general, **pero excluye explícitamente los Data Access logs** (según la documentación). Como el enunciado requiere específicamente acceso a **Data Access logs**, este rol **no cumple completamente** los requisitos — faltaría exactamente la categoría de logs que el equipo necesita ver.

---

#### La idea clave para el examen

Cuando el escenario pida acceso de **solo lectura a Data Access logs** (junto con Admin Activity y/o Access Transparency), la señal es clara: necesitas **`roles/logging.privateLogViewer`**, no `roles/logging.viewer` — porque este último **excluye explícitamente** los Data Access logs, mientras que `privateLogViewer` los incluye además de todo lo que ya cubre `viewer`. Recuerda esta distinción exacta: **viewer = todo excepto Data Access logs** | **privateLogViewer = todo + Data Access logs**.