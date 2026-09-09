![[Pasted image 20260908214637.png]]

#### Qué pide el escenario

- SCC ya **detectó y marcó** la anomalía (alto volumen de conexiones salientes a IPs diversas/desconocidas)
- Necesitas **investigar/confirmar** si la instancia realmente **fue comprometida por malware** — es decir, pasar de "señal sospechosa" a "diagnóstico confirmado"

---
#### Por qué D es correcta

> _"Analyze Event Threat Detection findings. Review the events and the outbound network connections associated with the instance."_

- **Event Threat Detection (ETD)** es el módulo de SCC diseñado específicamente para **detectar y correlacionar eventos de amenazas** dentro de tus logs (incluyendo actividad de red, comportamiento de malware, comunicación con C2, etc.), generando **findings enriquecidos con contexto** sobre la naturaleza de la amenaza.
- Al analizar los **findings de ETD** relacionados a esta instancia, y revisar los **eventos y conexiones salientes específicas**, puedes obtener evidencia concreta de **qué tipo de actividad maliciosa** está ocurriendo (por ejemplo, si ETD identifica las IPs de destino como infraestructura de C2 conocida, o detecta patrones de comportamiento típicos de malware) — esto te permite **confirmar o descartar** la hipótesis de compromiso con datos concretos, en lugar de solo la señal inicial genérica de "tráfico saliente inusual."
- Es el paso lógico de **investigación más profunda y específica** sobre la señal que SCC ya detectó — usando la misma familia de herramientas (SCC/ETD) diseñada exactamente para el análisis de amenazas de este tipo.

---
#### Por qué no las demás

**A** (Examinar roles IAM del service account y revocar permisos):

- Los **permisos IAM** no tienen relación directa con "**tráfico de red saliente**" — revisar y revocar permisos podría ser un paso de _remediación_ después de confirmar el compromiso, pero **no te ayuda a determinar** si la instancia fue comprometida por malware. Ataca una capa distinta del problema (control de acceso, no comportamiento de red/malware).

**B** (Revisar el Google Cloud Service Health dashboard):

- Este dashboard muestra **incidentes de la plataforma de Google Cloud en sí** (problemas de infraestructura de Google, interrupciones de servicio) — no tiene ninguna relación con analizar el **comportamiento específico de una VM individual**. Es una fuente completamente irrelevante para este diagnóstico.

**C** (Deshabilitar y volver a habilitar la interfaz de red):

- Esto es una acción de **"probar y ver si se resuelve"** sin ningún análisis real — no te dice **nada sobre la causa raíz** del tráfico anómalo. Si el malware sigue instalado en la VM, el tráfico probablemente **reaparecería** después de reactivar la interfaz, y de cualquier forma no aporta ninguna evidencia diagnóstica de si hubo o no compromiso. Es una acción reactiva sin valor investigativo.

---
#### La idea clave para el examen

Cuando SCC **ya generó una alerta/finding** sobre comportamiento sospechoso de una VM (especialmente relacionado a **red/tráfico**) y necesitas **confirmar si hay compromiso real**, la respuesta casi siempre apunta a profundizar en **Event Threat Detection** — es el módulo específico de SCC diseñado para correlacionar y dar contexto a este tipo de actividad, a diferencia de herramientas de gestión de accesos (IAM), monitoreo de infraestructura de Google (Service Health), o acciones de "prueba y error" sin base diagnóstica (reiniciar la interfaz de red).