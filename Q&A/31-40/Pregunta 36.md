![[Pasted image 20260908224531.png]]

#### Qué pide el escenario

- Fatiga de alertas causada por un **ejercicio de red team reciente** (actividad **conocida y ya identificada** como su propia fuente)
- Necesitas **filtrar específicamente los IOCs que sospechas vienen de ese ejercicio** — no de amenazas de baja confianza en general
- Objetivo: **reducir el tiempo** dedicado a revisar ruido

---

#### Por qué C es correcta

> _"Navigate to the IOC Matches page. Identify and mute the IOCs from the red team exercise."_

- La página **IOC Matches** es el lugar central en Google SecOps para revisar todos los IOCs que han sido **correlacionados automáticamente** contra tus datos UDM.
- La función de **"mute" (silenciar)** te permite **identificar puntualmente** los IOCs específicos que sabes que provienen del ejercicio de red team (ya que tú **conoces el contexto**: sabes qué actividad fue simulada) y **suprimirlos deliberadamente**, sin afectar la detección de amenazas reales no relacionadas con el ejercicio.
- Es la solución **más precisa y dirigida**: como ya tienes conocimiento específico de **qué actividad fue el ejercicio** (a diferencia de una amenaza desconocida), puedes identificar exactamente esos IOCs y silenciarlos de forma quirúrgica.

---

#### Por qué no las demás

**A** (Pedirle a Gemini una lista de IOCs del ejercicio de red team):

- Gemini no tiene **conocimiento del ejercicio de red team en sí** (una actividad interna/operativa de tu organización) — no es una fuente de verdad para saber qué IOCs específicos generó tu ejercicio. Gemini puede ayudar a generar código, resúmenes o reglas, pero no "sabe" qué pasó en tu red team a menos que se lo proporciones tú mismo.

**B** (Filtrar IOCs por tiempo de ingesta que coincide con el periodo del ejercicio):

- Aunque filtrar por **ventana de tiempo** parece razonable, es una aproximación **imprecisa**: podrías **excluir amenazas reales** que coincidieron casualmente con esa ventana de tiempo, o **dejar pasar IOCs del ejercicio** que se registraron ligeramente fuera de ese rango. No es tan preciso como identificar y silenciar los **IOCs específicos** conocidos del ejercicio.

**D** (Revisar IOCs con IC-Score >= 80%):

- Esto ataca un problema diferente: **filtrar por confianza del indicador en general** (como vimos en la pregunta 23, para reducir falsos positivos genéricos). Pero aquí el problema **no es que los IOCs sean de baja confianza** — de hecho, un red team exercise bien ejecutado podría generar IOCs con **alta confianza** (actividad que técnicamente se parece mucho a un ataque real, esa es la idea del red team). Filtrar por IC-Score alto **no necesariamente excluye** los IOCs del ejercicio, y podría **ocultar amenazas reales** de baja confianza que sí necesitan revisión. No resuelve el problema específico planteado.
#### La distinción clave con la pregunta 23 (mencionada anteriormente)

- **Pregunta 23**: problema genérico de "demasiados falsos positivos" sin causa conocida → solución: ajustar el **umbral de IC-Score** en las reglas.
- **Pregunta 36**: problema con **causa conocida y específica** (un red team exercise que tú puedes identificar) → solución: **identificar y silenciar (mute)** esos IOCs puntuales en la página de IOC Matches.


---
#### La idea clave para el examen

Cuando **ya sabes la causa exacta** del ruido (una actividad conocida como un red team exercise, un escaneo autorizado, etc.), la solución correcta es **identificar y silenciar (mute) los IOCs específicos** en la página de IOC Matches — es una acción **dirigida y consciente del contexto**, en lugar de depender de filtros genéricos por tiempo, umbrales de confianza, o herramientas de IA que no tienen visibilidad de tu actividad operativa interna.

