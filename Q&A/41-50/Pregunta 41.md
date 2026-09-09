![[Pasted image 20260908231148.png]]

#### Qué pide el escenario

- Alerta de **Container Threat Detection**: un **binario agregado** fue ejecutado en un workload de **alta criticidad para el negocio**
- Necesitas **investigar y responder** apropiadamente

---

#### Por qué B es correcta

> _"Review the finding, investigate the pod and related resources, and research the related attack and response methods."_

- Este es el paso **fundamental de investigación**: antes de tomar cualquier acción de contención drástica, necesitas **entender qué pasó realmente** — revisar los detalles del finding, examinar el pod afectado y los recursos relacionados (otros pods, servicios, configuraciones asociadas), y **investigar el tipo de ataque** (qué técnica se usó, qué respuesta es apropiada según el patrón de ataque conocido).
- Es el paso de **diagnóstico** que informa todas las decisiones posteriores — sin este análisis, cualquier acción de respuesta sería una suposición sin fundamento.

#### Por qué A es correcta

> _"Notify the workload owner. Follow the response playbook, and ask the threat hunting team to identify the root cause of the incident."_

- Como se trata de un **workload crítico para el negocio**, es esencial **notificar al dueño/responsable del workload** — esta persona tiene contexto operacional (por ejemplo, si hay actividad legítima de despliegue en curso, o el impacto de negocio de una posible interrupción).
- **Seguir el playbook de respuesta** asegura que el proceso de incident response siga los pasos **estandarizados y aprobados** por la organización, en lugar de improvisar.
- Involucrar al **equipo de threat hunting** para identificar la **causa raíz** es clave: no basta con reaccionar al síntoma (el binario ejecutado) — necesitas entender **cómo llegó ahí** ese binario (¿vulnerabilidad en la imagen? ¿acceso no autorizado al pipeline de CI/CD? ¿compromiso de credenciales?), para poder remediar la causa real y prevenir recurrencia.
- Esto refleja un proceso de respuesta a incidentes **maduro y completo**: comunicación con stakeholders + proceso estandarizado + análisis de causa raíz.

---

#### Por qué no las demás

**C** (Cuarentena del cluster + eliminar el pod inmediatamente):

- Esto es una acción de **contención drástica y prematura**: eliminar el pod **destruye evidencia forense** (memoria, estado del proceso, artefactos) antes de haber investigado. Además, poner en cuarentena **todo el cluster** (no solo el pod afectado) podría causar una **interrupción masiva e innecesaria** de un workload crítico de negocio, sin haber confirmado primero el alcance real del compromiso. Actuar sin investigar primero es exactamente el patrón que vimos como incorrecto en preguntas anteriores (ej: pregunta 39).

**D** (Silenciar la alerta asumiendo que es de baja severidad):

- Esto es **peligrosamente incorrecto**: el enunciado dice explícitamente que el workload es **"business critical"**, y que se trata de un **binario ejecutado** (no una configuración sospechosa menor) — esto amerita investigación seria, no ser descartado sin análisis. Asumir "baja severidad" sin evidencia es una negligencia de seguridad.

**E** (Dejar todo corriendo sin ninguna acción de contención mientras se investiga):

- Aunque la investigación es necesaria (como en B), **dejar el pod comprometido corriendo indefinidamente sin ningún tipo de contención o monitoreo intensivo** en un workload crítico es riesgoso — si el binario es efectivamente malicioso, podría seguir causando daño, moverse lateralmente, o exfiltrar datos mientras "investigas." No hay balance entre investigación y gestión de riesgo activo.

---

#### La idea clave para el examen

Ante una alerta de **Container Threat Detection en un workload crítico**, el proceso correcto combina: **investigación fundamentada** (revisar el finding, el pod, y el contexto del ataque — opción B) **+** un **proceso de respuesta estructurado y colaborativo** (notificar al dueño, seguir el playbook, involucrar threat hunting para la causa raíz — opción A). Esto evita los dos extremos problemáticos: **actuar de forma destructiva sin investigar (C)**, o **no actuar en absoluto asumiendo que no es grave (D)**, o dejar el riesgo corriendo indefinidamente sin ningún control (E).