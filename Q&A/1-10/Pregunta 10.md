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

- Esta es la **segunda capa, independiente:** el módulo **SOAR** (gestión de casos, playbooks)