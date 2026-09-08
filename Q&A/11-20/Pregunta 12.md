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