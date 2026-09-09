![[Pasted image 20260908230556.png]]


#### Qué pide el escenario

- Un requerimiento **automatizado y confiable**: cuando se cierra un caso **escalado**, el director del SOC debe recibir **automáticamente** un correo con los resultados
- Debe garantizarse que el email se envíe **"reliably"** (de forma confiable) — es decir, sin depender de que un analista **recuerde** hacerlo manualmente


---

#### Por qué D es correcta

> _"Create a playbook block that includes a condition to identify cases that have been escalated. The two resulting branches either close the alert and email the notes to the director, or close the alert without sending an email."_

- Esta es la única opción que propone una **automatización real e integrada al flujo de cierre del caso**: un **playbook block** con una **condición** que evalúa automáticamente si el caso fue escalado, y según el resultado, **ejecuta la rama correspondiente** (enviar email + cerrar, o solo cerrar).
- Al estar **integrado directamente en el proceso de cierre** (como parte del playbook que se ejecuta cuando se cierra el caso), garantiza que **cada vez que se cierre un caso escalado**, el email se envíe **sin depender de que un humano recuerde hacerlo** — cumpliendo el requisito de **confiabilidad** ("ensure the email is reliably sent").
- Es la solución **nativa de automatización** de SOAR: usa la lógica condicional del propio motor de playbooks para tomar la decisión correcta de forma consistente, cada vez.

---

#### Por qué no las demás

**A** (Usar el botón "Close Case" y, si es incidente, exportar y enviar el email manualmente):

- Esto depende **completamente de la memoria y disciplina del analista** — "si el caso es un incidente, exporta y envía" es un paso **manual y opcional en la práctica**. No hay ninguna garantía de que se haga siempre; es exactamente el tipo de proceso que **falla por error humano** (alguien lo olvida, tiene prisa, etc.). No cumple el requisito de "reliably."

**B** (Escribir un job que revise casos cerrados y envíe el email después):

- Aunque es automatizado, es un enfoque **reactivo y desacoplado**: revisa casos **después de que ya se cerraron**, en lugar de estar integrado en el momento del cierre. Esto introduce **latencia** (el director no se entera "antes" o en el momento del cierre, sino en algún ciclo posterior del job) y depende de programar/mantener un proceso externo (job) separado del flujo natural de trabajo en SOAR, cuando ya existe un mecanismo nativo (playbooks) diseñado para esto.

**C** (Navegar a Alert Overview, ejecutar una acción manual para recopilar detalles, y enviar el email manualmente si fue escalado):

- Igual que A, este es un **proceso completamente manual**: requiere que el analista **recuerde y ejecute cada paso** cada vez que cierra un caso. No hay automatización real ni garantía de consistencia — viola directamente el requisito de que el proceso sea **confiable y automático**.

---

#### La idea clave para el examen

Cuando el enunciado pida un proceso **"automático" y "confiable"** que dependa de una condición específica (como "si el caso fue escalado") en el momento de una acción concreta (cerrar el caso), la solución correcta casi siempre es un **playbook con lógica condicional (branching)** integrado directamente en el flujo — porque **elimina la dependencia del paso manual humano**, que es inherentemente menos confiable, sin importar cuán bien documentado esté el procedimiento manual.