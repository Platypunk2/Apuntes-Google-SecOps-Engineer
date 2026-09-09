![[Pasted image 20260909092459.png]]

### Qué pide el escenario

Eres ingeniero de seguridad en un **MSSP (Managed Security Service Provider)** — es decir, una empresa que gestiona la seguridad de **múltiples clientes** desde una sola plataforma de Google SecOps. Necesitas que los **casos de cada cliente estén separados lógicamente** entre sí. Esto es un requisito típico de arquitecturas multi-tenant: cada cliente debe tener sus propios casos, sin mezclarse con los de otros clientes.

La pregunta busca el mecanismo de **segmentación/separación estructural** dentro de SecOps SOAR, no solo un control de acceso o de flujo de trabajo.

---

### Por qué A es correcta: crear un "Environment" para cada cliente

En Google SecOps SOAR, el concepto de **Environment (Entorno)** es precisamente el mecanismo diseñado para la **segmentación lógica de datos/casos** entre distintos contextos organizacionales (por ejemplo, distintas unidades de negocio, regiones, o —como en este caso— distintos clientes de un MSSP).

- Cada **environment** actúa como un contenedor lógico separado: los casos, alertas y eventos que llegan asociados a un cliente específico pueden enrutarse y aislarse dentro de su propio environment.
- Es el mecanismo **nativo y recomendado** por Google para escenarios multi-cliente/multi-tenant como el de un MSSP.
- Permite además aplicar configuraciones, playbooks, y permisos de forma diferenciada por environment, reforzando la separación.

---

### Por qué las otras opciones NO son la mejor opción

**B. Crear un grupo de permisos por cliente**

- Los grupos de permisos controlan **quién puede acceder a qué** (control de acceso basado en roles/usuarios), pero **no separan lógicamente los casos en sí**.
- Sin un environment, los casos de distintos clientes seguirían mezclados en la misma estructura de datos; solo cambiarías quién puede verlos, no la organización lógica subyacente.

**C. Crear un playbook por cliente**

- Los playbooks son **flujos de automatización/respuesta** que se ejecutan sobre alertas o casos — no son un mecanismo de **organización o separación de casos**.
- Podrías tener playbooks personalizados por cliente, pero eso no resuelve el requisito de que los casos estén lógicamente separados entre sí.

**D. Crear un rol por cliente**

- Al igual que la opción B, un rol define **permisos y capacidades de un usuario** dentro de la plataforma (qué acciones puede realizar), no crea una **separación estructural de los datos/casos** de cada cliente.
- Es un control de acceso, no un mecanismo de segmentación de casos.