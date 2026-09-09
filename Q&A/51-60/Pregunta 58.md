![[Pasted image 20260909090353.png]]

No pude confirmar directamente el campo específico `principal.user.type` en la documentación, pero sí es un campo existente y reconocido en el esquema UDM de Google SecOps (usado para distinguir el tipo de identidad — usuario humano vs. cuenta de servicio). Vamos con el razonamiento de la pregunta:

#### Qué pide el escenario

- Alertas por **horarios de login inusuales** generando **muchos falsos positivos**
- La causa identificada: **cuentas de servicio** (service accounts) usadas por **tareas automatizadas programadas** — que naturalmente se ejecutan a "horas inusuales" (de madrugada, fuera de horario laboral) porque **no son humanos con horario de oficina**
- Necesitas usar **contexto a nivel de entidad** (entity-level context) disponible en SecOps, de la forma **más efectiva**

---

#### Por qué B es correcta

> _"Modify the rule to include the principal.user.type != 'service_account' condition."_

- El campo **`principal.user.type`** en el UDM de Google SecOps captura el **tipo de identidad** asociada al evento — permitiendo distinguir explícitamente entre un **usuario humano** y una **cuenta de servicio** (service account) directamente desde el contexto de la entidad enriquecida.
- Al agregar la condición `principal.user.type != "service_account"` directamente en la **regla de detección**, excluyes de raíz **todos los eventos de service accounts** de la lógica de "horario inusual" — esto es exactamente lo que se necesita, porque el concepto de "horario inusual" **no aplica de forma significativa** a cuentas de servicio (que operan 24/7 por diseño).
- Es la solución **más efectiva y directa**: usa un campo de **contexto de entidad ya existente y semánticamente correcto** (el tipo de usuario), aplicado **dentro de la lógica misma de la regla**, resolviendo la causa raíz del ruido de forma precisa.

---
#### Por qué no las demás

**A** (Usar asset tags para agrupar sistemas de automatización conocidos y excluirlos):

- Esto requiere **mantenimiento manual continuo**: cada vez que se cree una nueva automatización/service account, alguien tendría que **recordar taguearla** correctamente para que quede excluida. Es un proceso más frágil y dependiente de disciplina operativa, en comparación con usar un campo de tipo de entidad **ya estructurado y automático** como `user.type`.

**C** (Alertar solo cuando `principal.user.email` y `principal.user.userid` coincidan en el mismo evento):

- Esta condición **no tiene relación lógica** con el problema descrito (diferenciar humanos de service accounts). No hay ninguna razón por la que ese matching específico ayude a filtrar cuentas de servicio de tareas automatizadas — es una condición técnicamente posible pero **conceptualmente desconectada** del objetivo.

**D** (Reference list de todas las service accounts, y suprimir alertas por coincidencia en `principal.user.email`):

- Aunque podría funcionar, requiere **construir y mantener manualmente** una lista completa y actualizada de todas las cuentas de servicio — un proceso de **mayor esfuerzo continuo** comparado con usar el campo **`user.type`**, que ya clasifica esto de forma **nativa y automática** sin que tengas que enumerar cada cuenta individualmente. Cada vez que se cree una nueva service account, tendrías que **actualizar la lista manualmente**; con la condición de tipo, **cualquier** cuenta clasificada como service account queda excluida automáticamente, sin mantenimiento adicional.

---
#### La idea clave para el examen

Cuando el enunciado pida usar **"contexto a nivel de entidad"** para distinguir **tipos de identidad** (usuario humano vs. cuenta de servicio) de la forma **más efectiva**, la respuesta correcta apunta a usar **campos de clasificación ya existentes en el UDM** (como `user.type`) directamente en la condición de la regla — en lugar de soluciones que dependen de **mantenimiento manual de listas o tags** (opciones A y D), que son más frágiles y requieren actualización constante a medida que se crean nuevas cuentas.
