**Spanner Graph** es una **funcionalidad de Cloud Spanner** (la base de datos relacional destribuida de Google, conocida por su escalabilidad global) que permite modelar y consultar datos como un **grafo de propiedades** (property graph), sin necesidad de una base de datos de grafos separada.

---

# Conceptos clave:

- **Nodos:** representan entidades (ej: una VM, un usuario, una cuenta de servicio)

- **Aristas (edges):** representan las relaciones entre esos nodos (ej: "tiene acceso a", "está conecta a", "pertenece a")

- Usa **GQL** (Graph Query Language, estándar ISO) para hacer consultas de tipo "encuentra todos los caminos entre A y B" o "qué recursos están a 2 saltos de este servidor"

---
# Por qué es distinto de una base de datos relacional tradicional:

En SQL tradicional, para responder "¿qué recursos se conectan con este, y con qué se conecta eso, y así sucesivamente?" tendrías que hacer múltiples JOINs anidados — lento e ineficiente a medida que crece la profundidad. Spanner Graph está optimizado justamente para este tipo de **traversal (recorrido) de relaciones**, que es mucho más natural y rápido en un modelo de grafo.

---
# Por qué aplica a seguridad:

Google lo promueve específicamente para casos como: entender interdependencias entre dispositivos, usuarios y eventos para detectar anomalías, rastrear el origen de ataques, y evaluar el impacto de brechas correlacionando estos hallazgos con tendencias temporales — exactamente lo que se necesita para detectar **movimiento lateral** o comportamiento anómalo en un entorno cloud.