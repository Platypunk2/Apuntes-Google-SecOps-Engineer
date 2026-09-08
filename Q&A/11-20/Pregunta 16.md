![[Pasted image 20260908101911.png]]

**Qué pide el escenario**

Dos requisitos que deben cumplirse **simultáneamente:**

1. Los analistas de Company A **NO deben ver datos de casos** que vengan de fuera de Company A (aislamiento de datos)

2. Los analistas de Company A **SÍ deben poder reutilizar playbooks** ya creados por tu organización (no aislamiento total — hay recursos compartidos)

3. Todo esto con el **mínimo esfuerzo posible**

Este es exactamente el patón de **multi-tenancy dentro de una sola instancia** que describe la documentación: usar **Environments** como "contenedores lógicos" para segregar datos de casos, mientras que los playbooks pueden configurarse para ejecutarse **across multiple environments** (compartidos).

**Por qué el flujo lógico completo requiere ambas piezas**

Según la decumentación, el proceso de onboarding para un escenario multi-tenant/MSSP es:

1. **Definir un rol (SOC role) y grupos de permisos**

2. **Configurar un Environment** y asociarlo a los nuevos usuarios

3. Mapear usuarios a esos roles + environment + grupo de permisos

---

**Por qué la respuesta es C (definir un nuevo SOC Role), y no D (crear el Environment)**

- La documentación es clara: **"Google SecOps SOAR only: On the User Management page, create new users and assign the required environment, SOC role, and permission group."** Es decir, el rol SOC y el Environment van de la mano, pero el **rol** es el elemento que **define qué puede hacer** un usuario y **a qué tiene acceso** — es la pieza de control de acceso que finalmente determina si un usuario ve o no ve datos ajenos, y si puede o no reutilizar playbooks existentes.

- Crear un Environment (D) por sí solo no resuelve nada de acceso todavía — es solo el "contenedor" donde vivirán los casos de Company A. Necesitas definir el rol que se asignará a los analistas de Company A para que el sistema sepa qué permisos tienen dentro de ese contenedor (ver solo su environment, y tener acceso de uso — no necesariamente edición — sobre playbooks compartidos).

- Como el objetivo es minimizar esfuerzo, definir primero el rol (con los permisos correctos: acceso restringido a su propio environment + permiso de uso sobre playbooks existentes) es el paso fundacional que luego se asocia al environment y a los usuarios — es la pieza de la que dependen las demás configuraciones de control de acceso.

---

**Por qué no las demás**

**A**
- Esto es una **instancia completamente separada** (multi-instancia), lo cual va totalmente en contra del requisito de **minimizar esfuerzo**. Tendrías que duplicar toda la configuración, integraciones, y **no podrías reutilizar los playbooks existentes** fácilmente entre instancias separadas — contradice directamente el segundo requisito del enunciado.

**B**
- Una **service account** se usa típicamente para autenticación de integraciones/APIs (procesos automatizados), no para controlar el acceso de **analistas humanos** a datos de casos. No resuelve el problema de segregación de datos para usuarios del SOC.

**D**
- No es que esté mal — de hecho, **es un paso necesario** en el proceso completo — pero el enunciado pregunta específicamente por el **primer paso**, y según el flujo documentado de Google, **definir el rol y los grupos de permisos** es el prerequisito conceptual antes de asociarlo a un environment y a los usuarios. El rol es lo que define **las reglas de acceso** que luego se aplicarán dentro del environment.

---
**La idea clave para el examen**

Cuando la pregunta trate sobre **aislar datos de casos pero compartir recursos como playbooks** entre distintos grupos/clientes dentro de la misma instancia de SecOps SOAR, la solución de fondo es la combinación **SOC Role + Environment**. Presta atención a si la pregunta pide **el primer paso** de un proceso — en ese caso, la definición del **rol** (que establece los permisos) suele preceder a la creación/asociación del **environment** (que es el contenedor de datos).
