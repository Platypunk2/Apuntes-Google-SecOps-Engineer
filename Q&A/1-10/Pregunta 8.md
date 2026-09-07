![[Pasted image 20260906203756.png]]

La respuesta correcta es la D:

El enunciado pide algo muy específico: construir **programáticamente** un **estructura de datos tipo entidad** que permita **consultar las conexiones (relaciones) entre recursos.** Eso es literalmente la definición de un **grafo de propiedades** (nodos + relaciones/aristas).

- **Cloud Asset Inventory** ya tiene una **relationship table** que captura las relaciones entre recursos de Google Cloud (por ejemplo, qué VM pertenece a qué red, qué cuenta de servicio tiene acceso a qué recurso, etc.). Es la fuente de datos nativa de GCP para esto.

- **Spanner Graph**, según lo confirma la documentación de Google, está diseñado exactamente para modelar datos conectados, representando información como una red de nodos y aristas, donde los nodos simbolizan entidades como clientes, productos o ubicaciones, y las aritas muestran las conexiones entre esos nodos, capturando relaciones. Además, Google explícitamente promueve este caso de uso para seguridad: entender las interdependencias entre dispositivos, usuarios y eventos a través del tiempo es esencial para identificar patrones y anomalías, y con Spanner Graph los profesionales de seguridad pueden usar capacidades de grafos para rastrear el origen de ataques, evaluar el impacto de brechas de seguridad y correlacionar estos hallazgos con tendencias temporales.

- Al ingestar la relationship table de Cloud Asset Inventory dentro de Spanner Graph, obtienes exactamente lo pedido: una **estructura de datos de entidades consultable programáticamente,** optimizada para atravesar relaciones (justo lo necesario para detectar comportamiento anómalo o movimiento lateral entre recursos).
---

**Con respecto a las demás alternativas:**

- **A** (Bash script + gcloud CLI + CSV → BigQuery): Funciona para tener datos tabulares en BigQuery, pero BigQuery no esta optimizado para **consultas de traversal de grafos** (recorrer cadenas de relaciones tipo "¿qué recurso se conecta con cuál, y con cuál después?"). Es un enfoque de "fuerza bruta" manual, no una estrucutra de datos de entidad diseñada para este propósito.

- **B** (Asset Query tab + join con Cloud Asset Inventory → export a BigQuery): Es una opción de UI dentro de SCC, útil para consultas puntuales, pero termina en el mismo problema que A: BigQuery es relacional/tabular, no un modelo de grafo nativo. No construye una "entity data structure" optimizada para relaciones.

- **C** (Attack path simulation): Esto es una función de análisis (simular rutas de ataque/movimiento lateral usando datos ya existentes), no una forma de construir programáticamente la estructura de datos de entidades. Es un caso de uso posterior a tener los datos estructurados, no la solución al requisito planteado en la pregunta.

En resumen: la pregunta busca la herramienta correcta para **modelar relaciones como grafo de forma nativa y consultable,** y esa es exactamente la combinación **Cloud Asset Inventory relationship table + Spanner Graph** (opción D), mientras que las otra opciones usan herramientas relacionales/tabulares o quedan en una capa de análisis posterior.