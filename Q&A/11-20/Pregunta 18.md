![[Pasted image 20260908114116.png]]

**El escenario tiene dos decisiones independientes que verificar**

1. **¿Qué mecanismo de identidad usar?** (Google Group vs. Workforce Identity Pool)

2. **¿Qué rol de IAM otorgar?** (`chronicle.viewer` vs. `chronicle.limitedViewer` vs. `chronicle.editor`)

**Decisión 1: Google Group (no Workforce Identity Pool)**

El dato clave del enunciado es: **"Your organization uses Cloud Identity as their identity provider (IdP)"**.

- **Cloud Identity** es el propio servicio de identidad **de Google** (no es un proveedor externo/tercero).

- La **federación de identidad de la fuerza laboral (Workforce Identity Federation)**, que usa **workforce identity pools**, está diseñada específicamente para integrar IdPs de **terceros** (como Okta, Azure AD, Ping) con Google Cloud — **no es necesaria cuando el propio Cloud Identity ya es la fuente de identidad**.

- Cuando el IdP es Cloud Identity/Google Workspace, la práctica estándar es simplemente **crear un Google Group**, añadir los usuarios, y otorgar el rol de IAM directamente al grupo. No hay ninguna necesidad de la capa adicional de federación.

Esto **descarta automáticamente C y D**, porque ambas usan `workforcePools` — un mecanismo diseñado para IdPs externos, que no aplica aquí.

**Decisión 2: `roles/chronicle.viewer` (no `chronicle.limitedViewer`)**

Aquí está la diferencia exacta, confirmada por la documentación oficial de Google:

- **`roles/chronicle.viewer`**: otorga acceso de **solo lectura a la aplicación Google SecOps y a los recursos de la API**, sin ninguna exclusión — es el rol de lectura **completo**.

- **`roles/chronicle.limitedViewer`**: otorga acceso de solo lectura, pero **excluyendo explícitamente las reglas del motor de detección (detection engine rules) y los retrohunts**.

El enunciado pide explícitamente: **"read-only access to all resources, including detection engine rules**." Esa frase es la señal directa de que necesitas el rol **sin restricciones** — es decir, `chronicle.viewer`, no `chronicle.limitedViewer` (que justamente excluye lo que el enunciado pide incluir).

---

**Por qué no cada opción**

- **B** — Usa el mecanismo correcto (Google Group), pero el rol incorrecto (`limitedViewer`), que **excluye** las reglas de detección — contradice directamente el requisito.

- **C** — Usa Workforce Identity Pool (innecesario, ya que el IdP es Cloud Identity, no un tercero) **y** otorga `chronicle.editor`, que da permisos de **edición**, no de solo lectura — viola el requisito de "read-only".

- **D** — Usa Workforce Identity Pool (mismo problema que C) y aunque usa `limitedViewer` (rol de solo lectura), excluye las reglas de detección — falla en ambos criterios.


