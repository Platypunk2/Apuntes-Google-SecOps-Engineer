![[Pasted image 20260909001823.png]]


### Qué pide el escenario

Usas Security Command Center (SCC) con **Event Threat Detection (ETD)** habilitado. Quieres que ETD detecte intentos de **exfiltración de datos** desde buckets de Cloud Storage y datasets de BigQuery específicos (sensibles), pero al mismo tiempo debes **minimizar los costos de Cloud Logging**.

Aquí hay dos requisitos en tensión que debes balancear:

1. Habilitar los logs necesarios para que ETD pueda detectar exfiltración.
2. No generar logs innecesarios que aumenten el costo.

---

### Por qué A es correcta: solo "data read" audit logs, solo en los recursos designados

**Exfiltración de datos = alguien está leyendo/copiando datos hacia afuera.**

- La detección de exfiltración de ETD se basa fundamentalmente en **eventos de lectura de datos** (Data Access audit logs tipo "data read") — porque exfiltrar significa **leer/extraer** datos, no escribirlos.
- Los logs de **"data read"** para Cloud Storage y BigQuery capturan operaciones como `storage.objects.get` o consultas de BigQuery que acceden a los datos — exactamente lo que ETD necesita analizar para detectar patrones de exfiltración (ej. grandes volúmenes de lectura, lecturas desde ubicaciones inusuales, copias masivas, etc.).
- Habilitarlo **únicamente en los buckets/datasets sensibles designados** (no en todos los recursos de la organización) reduce drásticamente el volumen de logs generados, minimizando el costo.
- Esto cumple ambos requisitos: suficiente cobertura para que ETD detecte exfiltración, y costo mínimo al limitar el alcance.

---

### Por qué las otras opciones NO son la mejor opción

**B. "data read" Y "data write", solo en los recursos designados**

- Agregar **"data write"** aumenta el volumen de logs (y por tanto el costo) sin aportar valor para la detección de **exfiltración** específicamente.
- La escritura de datos (`data write`) es relevante para detectar modificaciones o inserciones no autorizadas, pero **no es la señal principal de exfiltración** — que es fundamentalmente sobre lectura/extracción de datos.
- Es información redundante para este objetivo puntual, lo que va en contra de "minimizar costos".

**C. "data read" y "data write" para TODOS los buckets y datasets de la organización**

- Esto es lo opuesto a minimizar costos: habilita logging para **todos** los recursos, no solo los sensibles designados.
- El volumen de logs sería masivo comparado con lo que realmente se necesita, generando el mayor costo de todas las opciones.
- Además, el escenario específicamente dice "designated sensitive buckets and datasets" — no toda la organización.

**D. VPC Flow Logs**

- Los VPC Flow Logs capturan información de **tráfico de red** (IP origen/destino, puertos, bytes transferidos) a nivel de red, no eventos de acceso a datos a nivel de aplicación/API.
- **No son la fuente de datos que usa ETD para sus detecciones de exfiltración** en Cloud Storage/BigQuery — ETD depende de los Data Access audit logs para eso.
- Sería un log adicional, costoso, y que no resuelve directamente el requisito de detección de exfiltración vía ETD.
