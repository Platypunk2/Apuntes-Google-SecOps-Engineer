![[Pasted image 20260908230146.png]]

#### Qué pide el escenario

- Alerta de **severidad media** (no crítica) por descarga inusualmente alta desde un bucket, fuera de horario laboral
- El usuario **tiene acceso legítimo** — no es un actor externo ni una cuenta comprometida confirmada
- La pregunta pregunta específicamente: **¿qué debes hacer PRIMERO?**

---

#### Por qué D es correcta

> _"Review user1's timeline in Google SecOps, focusing on network events and resource access immediately preceding the download anomaly."_

- Este es el paso correcto de **investigación inicial**: antes de tomar cualquier acción de contención o remediación, necesitas **entender el contexto completo** de lo que llevó a esta actividad.
- Al revisar el **timeline del usuario** (línea de tiempo de actividad) justo **antes** de la descarga anómala, puedes determinar factores clave como:
    - ¿Hubo un login sospechoso o desde una ubicación/dispositivo inusual antes de la descarga? (indicaría posible cuenta comprometida)
    - ¿El usuario estuvo realizando actividad de red o accediendo a otros recursos de forma consistente con su rol de "senior developer" trabajando tarde? (podría ser comportamiento legítimo)
    - ¿Hay algún otro evento correlacionado que sugiera compromiso vs. actividad normal fuera de horario?
- Esto te da la **evidencia necesaria para decidir el siguiente paso correcto** — sin este contexto, cualquier acción posterior sería una **suposición prematura**.

---

#### Por qué no A (la opción más tentadora)

> _"Run a playbook to suspend user1's bucket access, and review their user timeline."_

- El problema con A es el **orden de las acciones**: está **suspendiendo el acceso ANTES de investigar** completamente.
- Recuerda el dato clave del enunciado: es un **senior developer con acceso legítimo**, y la alerta es de **severidad media** (no alta/crítica). Suspender el acceso de un empleado legítimo **sin evidencia previa suficiente** podría:
    - Interrumpir innecesariamente el trabajo de un empleado que simplemente estaba trabajando tarde en un proyecto (falso positivo)
    - Generar fricción organizacional si resulta ser actividad legítima
- La **investigación (revisar el timeline) debe preceder a la contención**, no ejecutarse en paralelo o después de una acción disruptiva. Actuar primero y preguntar después no es el enfoque adecuado para una alerta de severidad **media** con un usuario de **acceso legítimo confirmado**.

#### Por qué no B

> _"Enrich the bucket entity with sensitivity labels and ACL data."_

- Esto es un paso de **preparación/enriquecimiento de contexto general del recurso** (bucket), útil para tener metadatos organizados a futuro, pero **no es investigación activa** del incidente específico que está ocurriendo ahora. No te ayuda a entender **qué hizo el usuario** ni por qué la descarga fue anómala.

#### Por qué no C

> _"Create a default detection rule to monitor future downloads, and add user1 to a high-risk watchlist."_

- Esto es una acción **orientada al futuro** (prevención/monitoreo continuo) — no responde a la necesidad **inmediata** de investigar **este incidente específico que ya ocurrió**. Además, agregar al usuario a una watchlist de alto riesgo **antes de investigar** podría ser prematuro y potencialmente injusto si resulta ser actividad legítima.


---
#### La idea clave para el examen

Cuando la pregunta enfatice **"qué debes hacer PRIMERO"** ante una alerta de **severidad media** con un usuario de **acceso legítimo confirmado**, la respuesta correcta casi siempre es **investigar/recopilar contexto antes de actuar** (revisar el timeline, correlacionar eventos previos) — en lugar de saltar directamente a **acciones de contención disruptivas** (suspender acceso) que podrían no estar justificadas todavía. La secuencia correcta de respuesta a incidentes es: **investigar → confirmar → luego contener/remediar**, no al revés — a menos que la severidad sea crítica y el riesgo de esperar sea inaceptablemente alto (lo cual no es el caso aquí, con severidad media).
