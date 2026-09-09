![[Pasted image 20260908232414.png]]

#### El detalle clave del enunciado

> _"...you have recently discovered that **process hashes are not reliably captured** across all endpoints due to an **inconsistent Sysmon configuration**."_

Esta es la pista central: **no puedes confiar en el hash como criterio principal de detección**, porque sabes que **muchos endpoints ni siquiera están capturando ese dato correctamente**. Si construyes tu detección basándote principalmente en el hash, vas a **perder detecciones** en todos los endpoints donde el Sysmon esté mal configurado — exactamente lo que quieres evitar cuando investigas un posible APT que ya podría llevar tiempo operando **sin ser detectado**.

#### Qué información SÍ tienes de forma confiable

1. Un hash SHA256 de una DLL maliciosa (**dato poco confiable**, según el enunciado)
2. Un dominio C2 conocido (dato de **red/DNS** — no depende de Sysmon)
3. Un **patrón de comportamiento**: `rundll32.exe` genera `powershell.exe` con argumentos ofuscados (dato de **relación de procesos**, que normalmente sí se captura de forma más consistente vía EDR, incluso si el hash específico falla)

---
#### Por qué A es correcta

> _"Write a multi-event YARA-L detection rule that correlates the process relationship and hash, and run a retrohunt based on this rule."_

- Una regla **multi-evento** te permite **correlacionar varias señales a la vez**: el patrón de comportamiento (rundll32 → powershell con argumentos ofuscados) **más** el hash cuando esté disponible — en lugar de depender de **una sola señal frágil**.
- Al **combinar múltiples indicadores** (comportamiento + hash + posiblemente el dominio C2), la regla se vuelve **resiliente a la limitación de datos** descrita: incluso si el hash falla en captarse en algunos endpoints, el **patrón de comportamiento** (que no depende de la configuración de Sysmon de la misma manera) puede seguir detectando la actividad sospechosa.
- El **retrohunt** es crucial porque el enunciado dice explícitamente que estás investigando si el actor **"ha operado sin ser detectado"** — necesitas aplicar la regla contra **datos históricos**, no solo hacia adelante, para descubrir actividad pasada que pudo haber pasado desapercibida.

---

#### Por qué no C (la opción "trampa" más directa)

> _"Create a single-event YARA-L detection rule based on the file hash, and run the rule against historical and incoming telemetry."_

- Esta opción **ignora completamente la advertencia del enunciado**: basar la detección **únicamente en el hash** es exactamente el enfoque **frágil** que fallará en los endpoints donde Sysmon no está capturando hashes correctamente.
- Es un distractor diseñado para quien no preste atención al detalle sobre la **inconsistencia de Sysmon** — parece razonable a primera vista (sí usa retrohunt-like históricos), pero la base de detección es la señal **menos confiable** de las tres disponibles.

#### Por qué no B

> _"Build a reference list with the hash and domain, and link it to a high-frequency rule for near real-time alerting."_

- Aunque usar una **lista de referencia** con IOCs (hash + dominio) es una práctica válida en general, esta opción **no incorpora el patrón de comportamiento** (rundll32 → powershell), que es la señal **más resiliente** dado el problema conocido con los hashes.
- Además, se enfoca en alertas **"near real-time"**, dejando de lado la necesidad explícita de investigar actividad **histórica no detectada** (no menciona retrohunt).

#### Por qué no D

> _"Use Google SecOps search to identify recent uses of rundll32.exe, and tag affected assets for watchlisting."_

- Esto es demasiado **genérico y ruidoso**: `rundll32.exe` es un proceso **legítimo del sistema operativo Windows**, usado constantemente para funciones normales. Buscar **todo uso reciente** sin la correlación específica con `powershell.exe` y argumentos ofuscados generaría una cantidad masiva de **falsos positivos**, sin aprovechar el patrón de comportamiento específico ni el contexto histórico que necesitas.

---

#### La idea clave para el examen

Cuando el enunciado te **advierta explícitamente sobre una limitación de datos** (como captura inconsistente de un campo específico), la respuesta correcta debe **evitar depender únicamente de esa señal débil**, y en su lugar **correlacionar múltiples indicadores** (especialmente patrones de comportamiento que sean más resilientes) mediante una **regla multi-evento**. Además, cuando se investigue actividad de un actor que pudo haber **operado sin ser detectado**, la presencia de un **retrohunt** contra datos históricos es casi siempre parte de la respuesta correcta.
