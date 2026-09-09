![[Pasted image 20260908205048.png]]

#### Cómo funciona la prioridad de playbooks en Google SecOps SOAR

Según la documentación oficial: puedes establecer la prioridad de un playbook entre **1 (más alta) y 3 (más baja)**, y **si hay múltiples playbooks adjuntos, el que tiene la prioridad más alta se ejecuta primero**. Además, el sistema **solo adjunta automáticamente un playbook** cuando hay varios triggers coincidentes — decide **cuál** basándose en el orden de prioridad.

---

#### Por qué A es correcta

> _"Set the priority of the 'All' playbook to a higher value than the priority of the specific playbook..."_

- Como el playbook "All" está pensado como un **catch-all/fallback** (debe aplicarse solo si **ningún otro playbook más específico** coincidió), necesitas que el sistema **evalúe primero los playbooks específicos**, y solo recurra al "All" si ninguno de esos aplicó.
- La documentación confirma este patrón exacto: el trigger "All" debería tener una **prioridad numérica más baja en importancia** (es decir, un **valor de prioridad más alto**, como **3**), mientras que los playbooks específicos deben tener una prioridad **más alta** (valor más bajo, como **1 o 2**). Así, cuando múltiples triggers coinciden, **el sistema elige automáticamente el playbook específico** por tener mayor prioridad, y el "All" solo se activa si no hay ningún otro playbook con mayor prioridad que haya coincidido.
- Esto es exactamente lo que dice la opción A: subir el **valor de prioridad** del playbook "All" (haciéndolo numéricamente mayor = **menos prioritario**) para que se evalúe **después** de los playbooks más específicos.

---

#### Por qué no las demás

**B** (Hacer el trigger "All" más preciso para que no dispare cuando el otro playbook es necesario):

- Esto **contradice el propósito mismo** del playbook "All": su función es ser un **catch-all genérico** que cubra cualquier caso no anticipado. Si lo haces "más preciso" para excluir casos específicos, dejas de tener una verdadera cobertura universal — tendrías que mantener manualmente una lista creciente de exclusiones cada vez que agregues un nuevo playbook específico, lo cual es frágil y con alto esfuerzo de mantenimiento.

**C** (Agregar un campo específico en la sección Outcomes de la regla de detección):

- Esto modifica la **regla de detección** en sí, no la lógica de **selección de playbooks** en SOAR. El problema descrito es de **priorización entre playbooks ya definidos**, no de qué datos extrae la regla de detección — es una solución que ataca la capa equivocada del problema.

**D** (Crear una regla de tagging y usar un tag trigger):

- Esto introduce una **capa de complejidad adicional** (nuevas reglas de tagging) cuando el sistema **ya tiene un mecanismo nativo diseñado exactamente para este propósito**: el sistema de prioridades de playbooks. Es una solución más compleja e indirecta para un problema que se resuelve de forma simple y directa ajustando la prioridad.

---

#### La idea clave para el examen

Cuando tengas un **playbook "catch-all" (trigger "All")** que debe actuar solo como **fallback**, la solución nativa y recomendada es ajustar su **prioridad numérica a un valor más alto** (menos prioritario, ej: 3) mientras los playbooks específicos usan valores más bajos (más prioritarios, ej: 1 o 2) — así el sistema garantiza que **siempre se intente primero el playbook más específico** antes de recurrir al genérico.