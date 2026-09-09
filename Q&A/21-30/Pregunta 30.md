![[Pasted image 20260908211850.png]]

#### Qué pide el escenario

- El playbook actual está **desactualizado** y no cubre este tipo de ataque (remote shell)
- Necesitas un **playbook nuevo y funcional**, desplegable **lo antes posible**
- Debe ser **usable por analistas junior** (es decir, debe ser claro, guiado, no depender de expertise avanzada)
- Debes usar **herramientas disponibles en Google SecOps** para **acelerar** la creación del playbook

---

#### Por qué B es correcta

> _"Use the playbook creation feature in Gemini, and enter details about the intended objectives. Add the necessary customizations for your environment, and test the generated playbook against a simulated remote shell alert."_

- **Gemini en Google SecOps** tiene una función específica de **generación de playbooks** a partir de una descripción en lenguaje natural de los objetivos — esto aprovecha directamente la herramienta nativa de la plataforma para **acelerar drásticamente** la creación de un playbook desde cero, cumpliendo el requisito de "as soon as possible."
- El flujo descrito en B sigue las **mejores prácticas correctas**: generar con Gemini → **personalizar** para el entorno específico de la organización → **probar** (test) el resultado contra un **alert simulado** de remote shell antes de desplegarlo en producción. Esto asegura que el playbook generado automáticamente sea **validado y funcional** antes de ponerlo en manos de analistas junior.
- Cumple los tres requisitos del enunciado: usa herramientas nativas de SecOps (Gemini), es rápido, y pasa por un proceso de validación antes del despliegue.

---

#### Por qué no C (la opción más "parecida" y tentadora)

> _"Use Gemini to generate a playbook based on a template from a standard incident response plan and implement automated scripts to filter network traffic based on known malicious IP addresses."_

- El problema de C está en la **segunda mitad**: agregar automáticamente **scripts que filtran tráfico de red basados en IPs maliciosas conocidas** es una acción de **remediación automática específica**, que:
    1. **No está validada/probada** antes de implementarse (a diferencia de B, que explícitamente prueba el playbook contra un alert simulado antes de considerarlo listo)
    2. Es una **suposición de solución técnica específica** (bloqueo por IP) que podría no ser la respuesta correcta a un remote shell — un remote shell podría no depender de una IP maliciosa "conocida" (podría ser tráfico legítimo comprometido, o una IP nueva no catalogada), haciendo que esa automatización específica sea insuficiente o incluso contraproducente.
    3. Salta directamente a **automatización de bloqueo** sin pasar por el proceso de personalización y prueba que sí tiene B — es más arriesgado desplegar algo así "ya asumido como correcto" en manos de analistas junior sin validarlo primero.

#### Por qué no A

> _"Add instruction actions to the existing playbook... have a senior analyst build out the playbook."_

- Esto depende de **trabajo manual de un analista senior** para actualizar el playbook — es **más lento** y no aprovecha ninguna herramienta de aceleración de Google SecOps (como Gemini), contradiciendo el requisito de usar herramientas disponibles para **streamline** el proceso.

#### Por qué no D

> _"Create a new custom playbook based on industry best practices, and work with an offensive security team to test..."_

- Aunque menciona "test" (que es positivo), **no usa ninguna herramienta de SecOps para acelerar la creación** — sería un proceso manual de diseño desde cero, más lento que aprovechar Gemini. Además, involucrar a un **equipo de seguridad ofensiva** para probar es un paso más pesado/formal de lo necesario, cuando el enunciado pide velocidad y no explícitamente un pentest formal.

---

#### La idea clave para el examen

Cuando el escenario pida **crear rápidamente un playbook nuevo usando herramientas nativas de SecOps**, la respuesta correcta casi siempre involucra **Gemini para la generación inicial**, seguido de **personalización** y **prueba contra un escenario simulado** — cuidado con las opciones que agregan automatización de remediación específica **sin ese paso de validación previa**, porque desplegar acciones automáticas no probadas (como en C) es un riesgo operacional, especialmente si el playbook será usado por analistas junior.