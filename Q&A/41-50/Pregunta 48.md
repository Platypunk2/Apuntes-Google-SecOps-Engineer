![[Pasted image 20260908234137.png]]

#### Qué pide el escenario

- Un finding de SHA: **CONFIDENTIAL_COMPUTING_DISABLED** — significa que una VM **no tiene habilitado Confidential Computing** (una protección de cifrado de datos en uso/en memoria)
- Necesitas **remediar rápidamente**

#### El contexto técnico clave

**Confidential Computing** es una configuración que se define **en el momento de creación** de la VM — no se puede simplemente "activar" en una VM que ya existe y está corriendo. Por eso, la remediación real requiere **eliminar la VM actual** y **recrearla** con Confidential Computing habilitado desde el inicio.

---

#### Por qué D es correcta

> _"Delete the offending VM instance, and allow the finding to be automatically marked as inactive."_

- Todas las opciones coinciden en el primer paso correcto: **eliminar la VM** (y presumiblemente recrearla con la configuración correcta habilitada) — esa es la **remediación técnica real** del problema.
- La diferencia está en **qué hacer con el finding después**: SCC/SHA tiene un mecanismo de **re-evaluación automática** — en el siguiente ciclo de escaneo, si el recurso que causó el finding **ya no existe o ya no presenta la condición de riesgo**, el sistema **marca automáticamente el finding como inactivo**, sin intervención manual.
- Esto es lo más **confiable y correcto**: dejas que la plataforma **verifique objetivamente** que el problema fue resuelto, en lugar de **forzar manualmente** un estado que podría no reflejar la realidad exacta del sistema.

---

#### Por qué no las demás

**A** (Eliminar la VM y **mutear** el finding):

- **Mute** (silenciar) es un mecanismo diseñado para **suprimir findings que consideras falsos positivos o aceptables como riesgo conocido** — no es la acción correcta cuando **sí hiciste una remediación real**. Mutear en este caso oculta el finding sin que quede reflejado como "resuelto correctamente", lo cual puede generar confusión en auditorías futuras (¿se resolvió, o se decidió ignorar el riesgo?).

**B** (Eliminar la VM y **deshabilitar el detector de SHA**):

- Esto es una medida **drástica y contraproducente**: deshabilitar el detector significa que **SCC dejará de monitorear esta condición en absoluto**, para **todas las VMs de tu fleet**, no solo la afectada. Perderías visibilidad futura sobre cualquier otra VM que tenga este mismo problema — un riesgo de seguridad significativo a cambio de "resolver" un solo finding.

**C** (Eliminar la VM y **marcar manualmente** el finding como inactivo):

- Aunque el resultado final (finding inactivo) es el mismo que en D, hacerlo **manualmente** es un paso **innecesario y menos confiable** que dejar que el sistema lo haga automáticamente tras su próximo ciclo de re-evaluación. Marcar manualmente introduce la posibilidad de **error humano** (marcar como resuelto antes de confirmar que realmente lo está) y no sigue el flujo de **verificación objetiva** que ofrece la plataforma de forma nativa.

---

#### La idea clave para el examen

Cuando remedies un finding de SCC/SHA de forma real (corrigiendo la causa raíz del problema, como en este caso recreando la VM con la configuración correcta), la mejor práctica es **dejar que la plataforma verifique y marque automáticamente el finding como inactivo** en su próximo ciclo de escaneo — evita usar **mute** (que es para riesgos aceptados/falsos positivos, no para remediaciones reales), evita **deshabilitar detectores** (que sacrifica visibilidad futura), y evita **marcar manualmente** cuando el sistema ya tiene un mecanismo automático y más confiable para confirmarlo.