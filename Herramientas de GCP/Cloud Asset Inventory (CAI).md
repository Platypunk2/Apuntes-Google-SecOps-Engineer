Es una herramienta **nativa de Google Cloud Platform (GCP)**, independiente de Security Command Center. Existe como servicio propio dentro de GCP, disponible para cualquier proyecto/organización, se use o no SCC.

Cloud Asset Inventory (CAI) es un servicio de Google Cloud que actúa como **inventario centralizado y en tiempo (casi) real de todos los recursos** de tu organización en Google Cloud.

---

# Que hace

- Mantiene un registro histórico de **metadatos** que tus recursos (VMs, buckets de Cloud Storage, cuentas de servicio, redes VPC, firewalls, roles de IAM, etc.)

- Guarda **snapshots** del estado de los recursos en distintos momentos en el tiempo (útil para auditorías: "¿como estaba configurado este recurso hace 3 meses?")

- Registra **políticas de IAM** aplicadas a cada recurso (quién tiene acceso a qué)

- Incluye una **relationship table** (tabla de relaciones) que documenta cómo se conectan los recursos entre sí — por ejemplo: una VM pertenece a una subred, la subred pertenece a una VPC, una cuenta de servicio está adjunta a una VM, etc.

---

# Para qué sirve

- Auditoría y cumplimiento (compliance)
- Análisis de seguridad (saber exactamente qué existe y cómo está conectado)
- Detección de cambios no autorizados
- Es la fuente de datos que Security Command Center usa para entender el estado de tus recursos