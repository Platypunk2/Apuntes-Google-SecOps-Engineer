![[Pasted image 20260907230028.png]]

**Qué pide el escenario**

- El problema no es solo "demasiadas alertas" en general, sino algo más específico: **"due to the high volume of alerts, some true positives might be missed"** — es decir, hay tanto ruido que las alertas realmente importantes **se pierden entre las demás**

- Se necesita **reducir falsos positivos** y **mejorar el "positive ratio"** de las alertas, dando contexto adicional

Esto describe un problema de **priorización**: no todas las alertas tienen el mismo impacto, y sin ese contexto, los analistas no pueden distinguir qué revisar primero.

---

**Por qué A es correcta**

- El **CMDB** contiene información sobre qué activos son críticos para la organización (servidores de producción, sistemas financieros, bases de datos sensibles, etc.) — esto es exactamente el concepto de **High-Value Assets (HVA)**.

- Al ingerir esta información en Google SecOps, las reglas de detección pueden **ajustar automáticamente la prioridad/risk score** de una alerta según **qué tan sensible es el activo afectado**.

- Esto resuelve directamente el síntoma descrito: si una alerta involucra un activo crítico, sube de prioridad y **no se pierde** entre el resto del volumen de alertas de bajo impacto. Mejora el "positive ratio" porque los analistas dedican su atención a las alertas que **realmente importan**, en lugar de tratar todas por igual.

---

**Por qué no las demás**

**B**

- Es un caso de uso **muy específico y de nicho**: identificar actores conocidos de foros de dark web.

- No tiene relación con el problema descrito (alto volumen de alertas, priorización, falsos positivos generales). No aporta ningún mecanismo para reducir ruido ni para destacar alertas críticas.

**C**

- Validar con IOCs ayuda a confirmar si un indicador (IP, dominio, hash) es **conocido como malicioso**, lo cual es útil para reducir falsos positivos técnicos.

- Pero **no resuelve el problema de priorización por volumen**: podrías seguir teniendo miles de alertas válidas (con IOCs confirmados) sin ninguna forma de distinguir cuáles son urgentes según el impacto al negocio.

- Además, muchas amenazas dirigidas usan infraestructura nueva que **no está en ningún feed de IOCs conocidos**, por lo que no ataca directamente el riesgo de que **true positives se pierdan por volumen** — que es el núcleo del problema planteado.

**D**

- Los TTPs son útiles para dar **contexto táctico** sobre cómo opera un atacante (ej: "uso de PowerShell para movimiento lateral"), y sirven para **enriquecer o clasificar** una alerta ya generada.

- No aportan un mecanismo de **priorización basada en el impacto/criticidad del activo afectado** — que es justamente lo que el enunciado necesita para evitar que alertas importantes se pierdan en el volumen.

---

**La idea clave para el examen**

Cuando el enunciado combine **"alto volumen de alertas"** + **"true positives podrían perderse"**, la señal apunta a un problema de **priorización**, no de exactitud de detección. La solución correcta suele ser ingerir **datos de criticidad de activos (HVA/CMDB)** para que el sistema **eleve automáticamente la prioridad** de alertas sobre activos sensibles — a diferencia de IOCs o TTPs, que mejoran la **calidad/contexto** de la detección pero no resuelven directamente el problema de que alertas importantes queden enterradas entre el ruido.