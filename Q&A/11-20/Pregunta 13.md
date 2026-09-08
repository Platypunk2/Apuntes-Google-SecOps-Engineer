![[Pasted image 20260907225331.png]]

**Qué pide el escenario**

1. Detectar un **patrón de comportamiento:** descargas de archivos **frecuentes,** desde un **workspace compartido,** en una **ventana de tiempo corta**

2. Asignar **risk scores más altos** cuando la anomalía se repite

Esto describe un patrón de **múltiples eventos correlacionados en el tiempo,** no un solo evento aislado.

**Por qué B es correcta**
	"Create a frequency-based YARA-L detection rule that assigns a risk outcome score and is triggered when multiple suspicious downloads occur within a defined time frame."

- Una **regla basada en frecuencia (frequency-based)** en YARA-L usa la sección `match` con una ventana de tiempo (ej: `match $user over 10m`) para **contar cuántas veces** ocurre un evento dentro de ese periodo — exactamente lo que necesitas para detectar "descargas frecuentes en una ventana corta".

- Puedes combinar esto con la sección `outcome` para calcular un **risk score dinámico**, que puede **incrementarse** según el número de ocurrencias (por ejemplo, usando `math.max()` o sumando puntos por cada evento adicional dentro de la ventana).

- Esto cumple **ambos requisitos** del enunciado: detecta el patrón de frecuencia y asigna un score que refleja la repetición de la anomalía.

---

**Por qué no las demás**

**A** (Flagear con el score más alto, sin importar el timeframe):

- Ignorar el time frame es un problema, porque el comportamiento sospechoso **depende del contexto temporal** — una descarga de archivo por sí sola no es anómala; lo que la hace sospechosa es que **muchas ocurren en poco tiempo**. Sin ventana de tiempo, no puedes distinguir actividad normal de un patrón de exfiltración.

- Asignar siempre el score más alto tampoco tiene sentido — generaría ruido y falsos positivos masivos, y no refleja "risk scores más altos para anomalías repetidas" (que implica una escala progresiva).

**C** (Regla de single-event con umbral de 24 horas):

- Una regla de **single-event** en YARA-L evalúa **un evento a la vez**, sin capacidad nativa de contar/agregar múltiples eventos relacionados en una ventana temporal corta.
- Aunque menciona "large number of files in 24 horas", una regla de single-event no está diseñada para capturar correlación entre múltiples eventos — for eso existen las reglas **multi-event / frequency-based**, que sí pueden usar la cláusula `match ... over` para agrupar y contar eventos relacionados.
- Además, 24 horas contradice el enunciado, que específica una **ventana de tiempo corta** (short time window), no un día completo.

**D** (Detecciones curadas por defecto + alertas automáticas para descargas individuales):

- Las **curated detections** son reglas predefinidas por Google, genéricas, no personalizadas a tu caso específico (un workspace compartido en particular).
- Alertar sobre **descargas individuales** (single file download events) genera muchísimo ruido, porque una descarga de archivo aislada es normal y esperado — no es indicador de nada sospechoso por sí sola. Vuelve a ignorar el elemento clave de "frecuencia/repetición" que pide el enunciado.

---

**Conceptos clave de YARA-L para recordar**

|Tipo de regla|Qué evalúa|Cuándo usarla|
|---|---|---|
|**Single-event rule**|Un solo evento a la vez|Detectar una condición específica en un evento individual (ej: "un solo login desde un país bloqueado")|
|**Multi-event / frequency-based rule**|Múltiples eventos relacionados dentro de una ventana de tiempo (`match ... over`)|Detectar patrones de comportamiento, repetición, umbrales (ej: "5 intentos fallidos de login en 5 minutos", "descargas frecuentes en poco tiempo")|

---

 **La idea clave para el examen**

Cuando el enunciado mencione palabras como **"frequent"**, **"repeated"**, **"multiple occurrences"**, **"within a short/defined time window"**, casi siempre apunta a una **regla YARA-L basada en frecuencia (multi-event)**, no a una regla de single-event — porque necesitas **contar y correlacionar eventos a través del tiempo**, no evaluar uno aislado.
