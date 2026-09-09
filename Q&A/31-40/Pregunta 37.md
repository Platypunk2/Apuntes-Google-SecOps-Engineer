![[Pasted image 20260908225257.png]]

#### Qué pide el escenario

- Ya hubo una **brecha de seguridad** confirmada
- Necesitas **aumentar la analítica de amenazas lo más rápido posible** (velocidad es el factor crítico)

---

#### Por qué A es correcta

> _"Enable curated detections to identify threats."_

- Las **curated detections** son reglas de detección **ya construidas, probadas y mantenidas por Google/Mandiant** — no requieren que tú escribas, pruebes o valides ninguna lógica desde cero.
- Simplemente **habilitarlas** (activar el rule pack correspondiente) te da **cobertura de detección inmediata** contra un amplio catálogo de patrones de amenaza conocidos, sin ningún tiempo de desarrollo.
- Es, por mucho, la opción de **menor tiempo de implementación** entre todas — literalmente es un "toggle" de activación, cumpliendo exactamente el requisito de **"as quickly as possible."**

---

#### Por qué no las demás

**B** (Diseñar reglas YARA-L basadas en casos de uso del Marketplace):

- Aunque usar casos de uso del Marketplace como **referencia/plantilla** acelera el proceso comparado con escribir desde cero, **"diseñar" reglas** todavía implica un trabajo de **adaptación, configuración y prueba** — no es tan instantáneo como simplemente **habilitar** algo que ya está listo para usarse (como en A).

**C** (Desarrollar reglas YARA-L enfocadas en threat intelligence):

- Esto requiere **desarrollo activo desde cero**: escribir la lógica, definir las secciones event/match/condition, probarla, y desplegarla. Es el enfoque de **mayor esfuerzo y tiempo** entre todas las opciones — exactamente lo opuesto a lo que pide el enunciado ("as quickly as possible").

**D** (Ingerir datos desde una plataforma de threat intelligence externa - TIP):

- Configurar una **nueva integración de ingesta** (conectar un TIP externo, configurar el feed, mapear campos, etc.) toma **tiempo de configuración inicial** — no es una solución inmediata. Además, ingerir más datos de threat intel **por sí solo no genera detecciones**; seguirías necesitando reglas (curadas o personalizadas) que consuman esos datos para que se traduzcan en alertas accionables.

---
#### La idea clave para el examen

Cuando el enunciado enfatice **velocidad/inmediatez** ("as quickly as possible", "rápidamente") para aumentar la capacidad de detección, la respuesta casi siempre apunta a **habilitar curated detections** — porque es la única opción que no requiere ningún desarrollo, prueba o configuración de infraestructura nueva; es funcionalidad **ya lista para usar** que Google mantiene y actualiza continuamente. Las opciones que implican "diseñar" o "desarrollar" reglas, o configurar nuevas integraciones de datos, siempre tomarán más tiempo que simplemente **activar** algo ya construido.