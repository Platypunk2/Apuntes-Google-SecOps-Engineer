![[Pasted image 20260908140409.png]]

**Que pide el escenario**

- Un reporte de SecOps se ejecuta **exitosamente** (SecOps dice "listo, terminé")

- Pero **no aparecen datos** en el dataset de BigQuery de destino

- El dataset **sí existe** (ya se descartó ese problema)

Esta combinación es una señal muy específica en GCP: cuando un proceso "se ejecuta bien" pero **no logra escribir datos en el destino**, casi siempre es un problema de **permisos IAM** — el proceso no falla de forma visible porque técnicamente completó su tarea, pero **no tenía autorización para escribir** en el recurso final.

---

**Por qué D es correcta**

- Cuando Google SecOps exporta datos a BigQuery, la operación real de **escritura** la realiza la **cuenta de servicio (service account) de Google SecOps** — no la cuenta del usuario que configuró el reporte. El usuario simplemente **programa** la exportación; es la identidad de servicio de SecOps la que **ejecuta la escritura de datos** en el dataset de destino.

- Para que esa cuenta de servicio pueda **insertar/escribir datos** en un dataset de BigQuery, necesita el rol **`roles/bigquery.dataEditor`** otorgado **específicamente sobre ese dataset** (o sobre el proyecto, pero el nivel más preciso/mínimo necesario es el dataset).

- Sin este permiso, SecOps "completa" el proceso de generar el reporte y **enviarlo hacia BigQuery**, pero la escritura final es **rechazada silenciosamente** por falta de autorización — de ahí que el reporte se vea "exitoso" en SecOps mientras el dataset permanece vacío.

---

**Por qué no las demás**

**A** (`roles/iam.serviceAccountUser` a la propia cuenta de servicio de SecOps):

- Este rol permite que una identidad **actúe como/impersonar** una cuenta de servicio — es útil cuando un recurso necesita "tomar prestada" la identidad de una service account para ejecutar acciones. No tiene relación con **permisos de escritura de datos en BigQuery**. No resuelve el problema descrito.

**B** (Establecer un retention period para el export de BigQuery):

- Esto configura **cuánto tiempo se conservan los datos** una vez que ya están en el dataset — es un ajuste de **ciclo de vida de los datos**, no de **si los datos logran llegar** al dataset en primer lugar. Es irrelevante para el síntoma descrito.


**C** (Otorgar `roles/bigquery.dataEditor` a la **cuenta del usuario** que programó el reporte):

- Este es el "distractor" clásico: parece lógico pensar que el usuario necesita permisos, pero **el usuario no es quien ejecuta la escritura real de datos** — solo configura/programa el reporte. La operación de exportación corre bajo la identidad de la **cuenta de servicio de SecOps**, no la del usuario. Otorgar el permiso a la persona equivocada no soluciona nada.

---

**La idea clave para el examen**

Cuando un proceso de exportación/integración entre servicios de Google Cloud **"se ejecuta exitosamente" pero los datos no aparecen** en el destino, casi siempre el problema es de **permisos IAM de la cuenta de servicio** que realiza la operación real —no de la cuenta del usuario que la configuró, ni de ajustes de configuración del propio destino (como retención). Hay que identificar **qué identidad ejecuta técnicamente la acción** y verificar que tenga el rol correcto sobre el recurso específico (en este caso, `bigquery.dataEditor` sobre el **dataset**, no sobre el proyecto en general para A/impersonation).