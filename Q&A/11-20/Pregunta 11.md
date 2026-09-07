![[Pasted image 20260907173735.png]]

**Conceptos clave de UDM `principal` vs `target`**

En el modelo UDM de Google SecOps, todo evento de red tiene dos "lados":

- `principal.port` = la entidad que **origina** la conexión/sesión (el lado "fuente")
- `target.port` = la entidad que **recibe** la conexión (el lado "destino")

Cada uno tiene su propio campo de puerto:

- `principal.port` -> puerto usado por el lado que origina el tráfico
- `target.port` -> puerto usado por el lado que recibe el tráfico

**Aplicando esto al escenario**

El backdoor **corre en el servidor** y **escucha en el puerto TCP 5555**. La pregunta pide identificar **tráfico que se origina desde ese servidor** (no tráfico dirigido hacia él). 

Piensa en el flujo de comunicación:

1. Un cliente/atacante se conecta al servidor comprometido en el puesto 5555 -> en ese evento, el servidor sería el **target** (con `target.port = 5555`)

2. Pero cuando el **servidor responde o envía tráfico saliente** desde ese backdoor (es decir, el servidor actúa como el **originador** de ese comunicación de vuelta), el puerto de origen (source port) usado es el **5555** — porque es el puerto en el que el proceso del backdoor está escuchando/operando.

3. En ese caso, el servidor pasa a ser el `principal` del evento, y su puerto (5555) queda registrado en `principal.port`

Como la pregunta pide específicamente identificar **tráfico originado desde el servidor** (no tráfico entrante hacia él), necesitas filtrar por el campo que representa el **lado que origina** la comunicación -> `principal-port = 5555`. **Respuesta C**

---



