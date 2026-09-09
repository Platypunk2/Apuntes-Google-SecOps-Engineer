![[Pasted image 20260908234949.png]]

#### Qué pide el escenario

- Un problema **consistente**: los logs llegan con **6 horas de retraso** — al investigar, determinas que es un problema de **zona horaria en el timestamp del log**
- Necesitas **corregir** esto

---

#### Por qué B es correcta

> _"Create a parser extension to correct the time zone."_

- Según la documentación oficial, Google SecOps usa **parsers por defecto (default parsers)** — instrucciones de mapeo **prediseñadas y mantenidas por Google**, que **no puedes modificar directamente**. Google las actualiza mensualmente y son de solo lectura para el cliente.
- Los **parser extensions** son el mecanismo **soportado y diseñado específicamente** para **extender/ajustar** el comportamiento de un parser por defecto (o personalizado) **sin reemplazarlo por completo** — permitiéndote agregar lógica de mapeo adicional, transformar campos, o **corregir/ajustar valores específicos** (como aplicar el offset correcto de zona horaria al timestamp).
- Es la solución de **menor esfuerzo y mantenimiento**: sigues beneficiándote de las actualizaciones automáticas del parser por defecto de Google, mientras corriges puntualmente el problema específico de zona horaria mediante la extensión.

---

#### Por qué no las demás

**A** (Modificar el parser por defecto y agregar una zona horaria por defecto):

- Esto es **técnicamente imposible/no soportado**: los **parsers por defecto son gestionados por Google** y no están diseñados para ser editados directamente por el cliente. Cualquier intento de "modificarlos" no es la vía soportada — para eso existen precisamente los **parser extensions**.

**C** (Crear un parser personalizado (custom parser) para corregir la zona horaria):

- Aunque es **técnicamente posible**, esto es una solución de **mucho mayor esfuerzo**: un custom parser **reemplaza completamente** la lógica del parser por defecto, lo que significa que **pierdes las actualizaciones automáticas** que Google aplica mensualmente a ese log type, y **tendrías que mantener tú mismo toda la lógica de parsing** desde cero (no solo el ajuste de zona horaria). Es la opción reservada para cuando **no existe un parser por defecto**, o cuando deliberadamente **quieres optar por no recibir actualizaciones** — no es la solución adecuada para un ajuste puntual como este.

**D** (Modificar la configuración de la UI para corregir la zona horaria):

- No existe una configuración de UI que pueda **corregir el timestamp real de un evento a nivel de ingesta/parsing** — la zona horaria incorrecta es un problema de **cómo se interpreta el dato crudo del log** durante el parsing, no una preferencia de visualización en la interfaz. Ajustar configuraciones de UI no tiene ningún efecto sobre el `metadata.event_timestamp` real almacenado.

---

#### La idea clave para el examen

Cuando necesites **corregir o ajustar un campo específico** (como el timestamp/zona horaria) de un log type que **ya tiene un parser por defecto funcionando**, la solución correcta y de menor esfuerzo es siempre un **parser extension** — te permite personalizar puntualmente sin perder los beneficios (mantenimiento, actualizaciones automáticas) del parser por defecto de Google. Reserva el **custom parser** solo para casos donde **no existe parser por defecto**, o cuando necesitas **control total y permanente** sobre la lógica de parsing (renunciando deliberadamente a las actualizaciones automáticas de Google).