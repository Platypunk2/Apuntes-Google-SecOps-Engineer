EDR es una categoría de herramientas de seguridad que se instalan **directamente en los endpoints** (laptops, servidores, estaciones de trabajo) mediante un **agente** — un pequeño software que corre continuamente en segundo plano — para **monitorear, detectar y responder** a actividad maliciosa **en tiempo real**, a nivel del propio dispositivo.

---

**Qué hace, en detalle**

**Detección (Detection):**

- Monitorea procesos en ejecución, cambios en archivos, conexiones de red, modificaciones de registro (en Windows), llamadas al sistema, etc.

- Usa comportamiento (behavioral analysis), firmas de malware, machine learning, y reglas para detectar actividad sospechosa — no solo malware conocido, sino comportamientos anómalos (ej: un proceso de Word que de repente intenta ejecutar PowerShell y conectarse a internet)

**Respuesta (Response):**

- Permite tomar acciones directamente sobre el endpoint, como:

	- **Cuarentena/aislamiento** (quarantine/isolate): desconecta el equipo de la red pero lo mantiene encendido, preservando el estado del sistema
	
	  La función de **"quarantine"** de un EDR es especial porque:
		
		1. **Aísla la máquina** (corta su capacidad de comunicarse con el exterior, deteniendo exfiltración o comando-y-control)
		
		2. **NO apaga ni reinicia el sistema** — lo deja "congelado" en su estado actual
		   
		3. Esto preserva **evidencia forense volátil** (memoria RAM, procesos activos, conexiones abiertas) que se perdería si reinicias o pagas el equipo
		   	
	- **Matar procesos** maliciosos
	  
	- **Eliminar o poner en cuarentena archivos** especificos
	
	- **Recolectar evidencia forense** (memoria, logs, snapshots del sistema)
	  
	- **Rollback:** revertir cambios hechos por el malware (algunas soluciones avanzadas)
	  
---

**Ejemplos de herramientas EDR conocidas:**

- **CrowdStrike Falcon**

- **Microsoft Defender for Endpoint**

- **SentinelOne**

- **Carbon Black (VMware)**

- **Google's Chronicle Endpoint** / integraciones con terceros dentro de SecOps SOAR
