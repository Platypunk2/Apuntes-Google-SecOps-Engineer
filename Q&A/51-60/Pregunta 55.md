![[Pasted image 20260909001558.png]]
### Qué pide el escenario

Tienes una instancia de Compute Engine con una **imagen administrada por Google** (Google-managed image), que ya viene con el **Ops Agent** integrado. Necesitas llevar los logs de la aplicación a Google SecOps. El log ya es un formato estándar con un **parser y label válidos** en SecOps (es decir, no necesitas construir un parser personalizado). El objetivo principal es **minimizar costo y tiempo** de implementación.

Esto es clave: como ya tienes Ops Agent disponible de forma nativa (viene con la imagen), la solución más simple y de menor esfuerzo es aprovechar esa integración existente, no añadir componentes extra.

---

### Por qué A es correcta

**Usar el Ops Agent (ya embebido) → Cloud Logging → ingestión directa a SecOps**

- El Ops Agent **ya está integrado** en la imagen de Google, así que no hay que instalar ni mantener nada adicional (cero esfuerzo extra de despliegue).
- Cloud Logging es el destino nativo del Ops Agent — la ruta más directa y de menor fricción.
- Google Cloud tiene un **mecanismo de ingestión directa (Feed) entre Cloud Logging y SecOps**, diseñado justamente para este caso: mover logs de GCP hacia SecOps sin pasos intermedios como Cloud Storage o scripts personalizados.
- Como el log ya tiene parser y label válidos en SecOps, no necesitas transformar nada — solo conectar el feed.
- Es la opción de **menor costo y menor tiempo de implementación**, cumpliendo exactamente el requisito del escenario.

---
### Por qué las otras opciones NO son la mejor opción

**B. Desplegar un agente Bindplane**

- Bindplane es una alternativa válida para logs que **no** tienen ya una ruta nativa simple, o para casos más complejos/heterogéneos.
- Aquí sería **redundante**: ya tienes el Ops Agent disponible de forma nativa. Añadir Bindplane implica instalar, configurar y mantener un componente adicional — más costo y tiempo, no menos.

**C. Crear un script personalizado con la Ingestion API**

- Requiere desarrollo, mantenimiento y pruebas de un script customizado.
- Es la opción con **más esfuerzo de ingeniería**, totalmente en contra del objetivo de minimizar tiempo y costo. No aprovecha ninguna herramienta nativa existente.

**D. Ops Agent → Cloud Storage bucket → Feed en SecOps**

- Introduce un **paso intermedio innecesario** (Cloud Storage).
- Esto añade **latencia** (los logs no llegan en tiempo cercano al real, dependen de cómo se escriban/lean del bucket) y **costo adicional** de almacenamiento y operaciones de Storage.
- Cuando existe una ruta de ingestión directa desde Cloud Logging, pasar por Cloud Storage es un paso extra que no aporta valor aquí — solo se usaría si la fuente no soportara ingestión directa.