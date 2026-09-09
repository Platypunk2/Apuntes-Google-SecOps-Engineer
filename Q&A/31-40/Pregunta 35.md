![[Pasted image 20260908223724.png]]

#### El concepto clave: la segregación de datos ocurre por Environment, no por Role

La documentación es explícita: **los Environments logran la segregación de datos** — casos, alertas y entidades ingeridas en un environment quedan **lógicamente aislados** de otros, y los usuarios **solo pueden ver e interactuar con datos que residen en los environments a los que tienen acceso asignado**.

Este es el punto que hace que la pregunta 35 tenga sentido: el **Role** (Tier 1, Tier 2, Tier 3) define **qué puede hacer** un usuario dentro de la plataforma, pero **no es el mecanismo que aísla qué casos puede ver**. Ese aislamiento se logra específicamente a través de **Environments**.


---

#### Por qué C es correcta

> _"Configure the Cross Environment Policy to allow users to move cases between environments. Move Tier 3 cases to an environment that only Tier 3 analysts can access."_

- Actualmente, **todos los analistas ven todos los casos** — esto significa que probablemente todos comparten el **mismo environment** (o tienen acceso a los mismos environments).
- Para lograr que los **casos asignados al rol Tier 3 solo sean visibles para Tier 3**, necesitas **mover esos casos a un environment separado**, y configurar el acceso de ese environment para que **solo los analistas Tier 3 tengan permiso de verlo**.
- La **Cross Environment Policy** es justamente la configuración que **permite mover casos entre environments** — es el mecanismo habilitador necesario para ejecutar esta reestructuración.
- Esto logra el aislamiento real: aunque Tier 1 y Tier 2 sigan teniendo su rol y sus permisos normales, **ya no tendrán acceso al environment** donde residen los casos de Tier 3 — logrando el requisito exacto del enunciado.

---
#### Por qué no las demás

**A** (Instruir a Tier 1/Tier 2 a crear un filtro de cola de casos para excluir los de Tier 3):

- Esto es una solución **voluntaria y no forzada**: un filtro de vista es solo una preferencia personal de cómo un analista organiza lo que ve — **no impide técnicamente** que ese mismo analista pueda buscar, abrir o acceder al caso si quisiera. No es un control de acceso real, es solo una conveniencia de UI. No cumple el requisito de **restricción real de acceso**.

**B** (Revocar acceso de rol adicional de Tier 1 y Tier 2):

- Esto es vago y no ataca el mecanismo correcto: el problema no es que Tier 1/Tier 2 tengan "permisos de rol adicionales" no relacionados — es que **todos comparten el mismo espacio de datos (environment)**. Revocar "role access" no resuelve la segregación de **casos específicos**, porque el control de qué casos se ven está gobernado por **environments**, no directamente por ajustes de permisos de rol sueltos.

**D** (Asignar los casos a un usuario del rol Tier 3):

- Asignar un caso a un usuario específico como **responsable/dueño** no significa que **otros usuarios pierdan la capacidad de verlo** — en la mayoría de plataformas SOAR (incluyendo esta), la asignación de un caso a alguien no equivale a **restringir la visibilidad** para el resto del equipo. Los demás analistas seguirían pudiendo ver el caso en la cola general, solo que no serían el "asignado."

---
#### La idea clave para el examen

Cuando la pregunta trate sobre **restringir qué casos puede VER un grupo específico de analistas** (no solo qué acciones pueden realizar), la solución de fondo casi siempre involucra **Environments** — porque es el mecanismo nativo de **segregación real de datos** en Google SecOps SOAR. Los roles definen **capacidades/permisos de acción**, pero el **aislamiento de qué casos son visibles** se logra moviendo esos casos a un environment restringido y configurando el acceso a ese environment específicamente para el grupo que debe verlo.