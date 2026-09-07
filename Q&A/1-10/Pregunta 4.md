![[Pasted image 20260906182746.png]]![[Pasted image 20260906182758.png]]

La respuesta correcta es la **A** porque combina dos filtros necesarios para aislar específicamente los IoCs de dominio provenientes del feed de threat intelligence (MISP):

1. `$ioc.graph.metadata.entity_type = "DOMAIN_NAME"`
	Esto filtra el tipo de entidad para que solo se consideren entidades que son **dominios** (y no IPs, hashes de archivo, URLs, etc.). Como la regla busca específicamente indicadores de C2 basados en consultas DNS, necesitas asegurarte de que la entidad de grafo sea de tipo dominio.
	
2. `$ioc.graph.metadata.source_type = "ENTITY_CONTEXT"`
	Este es el punto clave que distingue la respuesta correcta de las demás. En el modelo de entidades de Google SecOps, el campo `source_type` indica **de dónde proviene la información sobre esa entidad:**
	
	- **ENTITY_CONTEXT**: datos de contexto/enriquecimiento sobre una entidad que provienen de una fuente externa como un feed de threat intelligence (justo el caso de MISP en este escenario). Es la categoría usada para indicadores de compromiso (IOCs) ingeridos vía feeds.
	
	- **GLOBAL_CONTEXT** (opción B): se refiere a contexto global proporcionado por Google/Chronicle, no a feeds de terceros que tú mismo integras.
	
	- **DERIVED_CONTEXT** (opción C): se refiere a contexto derivado internamente a partir de eventos UDM (no de un feed externo).
	
	- **SOURCE_TYPE_UNSPECIFIED** (opción D): valor genérico/no definido, no filtra nada útil.
	

**Por qué importa en este escenario**

El enunciado dice explícitamente que el *threat intel feed* fue ingerido **mediante integración nativa con MISP.** Los datos que llegan de esta forma se clasifican como "contexto de entidad" (entity context) dentro del grafo de entidades de SecOps. Por eso, para filtrar correctamente los dominios C2 que vienen de ese feed, necesitas `entity_type = "DOMAIN_NAME"` y `source_type = "ENTITY_CONTEXT"` juntos — ambos deben cumplirse para que `$ioc` represente exactamente lo que buscas: un IOC de tipo dominio, proveniente de un feed de inteligencia de amenazas.