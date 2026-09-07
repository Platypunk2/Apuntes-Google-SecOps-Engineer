![[Pasted image 20260907142506.png]]

**El punto clave del escenario**

El síntoma dice algo muy específico: **el equipo SOC puede autenticarse en la IdP** (es decir, el login/SSO funciona), **pero no está autorizado para acceder a la instancia**. Esto es una pista de que el problema es **de autorización, no de autenticación** — y en Google SecOps, la autorización funciona con un **modelo dual**: una capa a nivel de Google Cloud IAM, y otra separada a nivel de la aplicación SOAR.

---

**Por qué C y E son la respuesta correcta**

**C. Grant the `roles/chronicle.viewer` role to the SOC team's IdP group in IAM**

- Esta es la capa de **Google Cloud IAM**: cuando usas federación de identidad (Workforce Identity Federation) con un IdP de terceros, los usuarios/grupos federados se mapean a un `principalSet` en IAM.

- Ese `principalSet` necesita **al menos** el rol `roles/chronicle.viewer` para poder **iniciar sesión** en la instancia de SecOps. Sin este rol, el usuario se autentica en el IdP pero Google Cloud le niega el acceso a la instancia.

- Es el rol **mínimo predefinido** requerido para obtener acceso de firma/entrada (sign-in access) a la plataforma.

**E. Grant the Basic permission to the appropriate IdP groups in the Google SecOps SOAR Advanced Settings**

- Esta es la **segunda capa, independiente:** el módulo **SOAR** (gestión de casos, playbooks) tiene su **propio sistema de control de acceso interno**, separado de IAM.

- Aunque el usuario ya tenga `chronicle.viewer` en IAM, eso **no le da automáticamente permisos dentro de SOAR.** Un administrador debe ir a **SOAR Advanced Settings -> Users & Groups**, y asignar explícitamente un rol SOAR (Como "Basic" o "Analyst") al mismo grupo de IdP federado

- Sin este paso, el usuario podría entrar a la plataforma pero no podría trabajar con casos ni playbooks.

**Por qué se necesitan ambas (y no solo una)**

Este es el punto central del diseño de la pregunta: **Google SecOps tiene doble autorización**  — una para el **SIEM/Chronicle** (vía IAM) y otra para el **SOAR** (vía su propio sistema de roles). El equipo SOC necesita **ambas** para tener acceso funcional completo. Si solo aplicas C, entran a la plataforma pero no pueden operar en SOAR. Si solo aplicas E, tienen permisos configurados en SOAR pero ni siquiera pueden iniciar sesión porque IAM se los bloquea antes.

---

**Por qué no las demás**

- **A** (Link a un proyecto GCP con Chronicle API): esto es parte de la **configuración inicial** de la instancia, no de la autorización de usuarios específicos. El problema no es que la instancia no esté vinculada — de hecho, los administradores ya tienen acceso, lo cual confirma que esto ya está bien configurado.

- **B** (Data access scope en IAM): los **data access scopes** controlan **qué datos/logs** puede ver un usuario (ej: restringir por región, entorno, tipo de log) — no controlan si el usuario puede **acceder o no** a la instancia en absoluto. Es un refinamiento posterior, no la causa raíz del problema descrito.

- **D** (Workforce Identity Federation): esto ya está implícito como **ya configurado**, porque el enunciado dice explícitamente que los usuarios **ya logran autenticarse contra el IdP de terceros.** Si la federación no existiera, ni siquiera podrían autenticarse. El problema está un paso **después** de la autenticación — en la autorización.

---

**La lección clave para el examen**

Cuando veas un escenario de **"se autentican pero no están autorizados"** en Google SecOps con **IdP de terceros**, casi siempre la respuesta correcta involucra **el par:** rol de IAM (`chronicle.viewer` o similar) + configuración de permisos dentro de **SOAR Advanced Settings** — porque son dos sistemas de autorización independientes que deben configurarse por separado.
