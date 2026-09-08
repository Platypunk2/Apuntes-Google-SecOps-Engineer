![[Pasted image 20260907191841.png]]

**Que pide exactamente el enunciado**

1. **Contener la amenaza inmediatamente** (immediate containment)

2. **Preservar los datos forenses** para investigación posterior (eso es clave — no puedes destruir evidencia)

3. Hay indicios de **persistencia** ya instalada (no es solo tráfico sospechoso, sino que el atacante probablemente ya tiene forma de mantener acceso incluso si cortas las conexón de red)

---

**Por qué C es correcta**
	
	"Use the EDR integration to quarantine the compromised asset."
- **EDR (Endpoint Detection and Response)** actúa **directamente sobre el endopoint/servidor comprometido**, no solo sobre el tráfico de red.

- La función de **"quarantine"** en una herramienta EDR normalmente:
	
	- Aísla la máquina a nivel de red (bloquea toda comunicación excepto con la consola de administración/EDR)
	
	- **Mantiene la máquina encendida y en su estado actual** — no la apaga ni reinicia
	  
	- Preserva la memoria, procesos en ejecución, y el sistema de archivos **intactos** para el análisis forense posterior
	  
- Esto responde exactamente a los dos requisitos: **contención inmediata** (corta la capacidad del ataque de seguir operando/exfiltrando) y **preservación forense** (no se pierde evidencia volátil como memoria RAM, procesos activos, conexiones de red en curso).

---
**Por qué no las demás**

**A** (Bloquear la IP en el firewall):

- Esto solo bloquea el **tráfico de red específico hacia/desde esa IP** — no contiene la amenaza en el propio servidor comprometido.

- El enunciado menciona que **sospechan de mecanismos de persistencia ya instalados.** Si el atacante ya tiene persistencia (ej: tarea programada, servicio malicioso, backdoor local), bloquear solo una IP no detiene nada — el malware sigue corriendo en el servidor, podría comunicarse con otra IP/dominio, o seguir dañando el sistema localmente.

- Es una medida **parcial e insuficiente** para el nivel de amenaza descrito.

**B** (Desplegar parches de emergencia y reiniciar el servidor):

- Esto es **catastrófico para la forense:** reiniciar un servidor comprometido **destruye evidencia volátil crítica** — memoria RAM, procesos en ejecución, conexiones de red activas, artefactos temporales — todo lo que un investigador forense necesitaría analizar.

- Va directamente en contra del requisito explícito de **"ensuring that forensic data remains available"**.

- Además, parchear no necesariamente elimina persistencia ya isntalada (backdoors, cuentas creadas, tareas programadas).

**D** (Usar VirusTotal para enriquecer la IP, obtener el dominio, y bloquearlo en el proxy)

- Esto es una acción de **enriquecimiento/investigación**, no de **contención**.

- Aunque es útil como parte de la investigación (entender mejor la amenaza), **no detiene nada de forma inmediata** en el servidor comprometido — el enunciado pide contención **inmediata**, y esta opción ni siquiera actúa sobre el activo afectado.

- Similar a A, se enfoca solo en el tráfico de red/dominio, ignorando que la amenaza ya podría estar **dentro** del servidor mismo.

---

**La idea clave para recordar**

Cuando el escenario menciona **"persistence machanisms"** + **"preserve forensic data"** + **"immediate containment"**, la respuesta casi siempre apunta a una herramienta de **EDR con función de quarantine** — porque es la única que actúa **directamente sobre el endpoint comprometido** sin destruir el estado del sistema, a diferencia de bloqueos de red (insuficientes ante persistencia local) o reinicios/parches (que destruyen evidencia forense).